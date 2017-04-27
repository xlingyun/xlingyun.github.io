---
layout: post
title: "Homebrew总结"
date: 2017-04-26 22:17:29 +0800
comments: true
categories: 
---
[HomeBrew][brew]就是MacOS下最著名的包管理器，我使用它的目的就是让MacOS下的命令行和Linux下一样顺手。

* 安装
* 基本操作
    * 安装一个包
    * 卸载与跟新
* 扩展
* 更换源
* Homebrew中一些路径的解释
* MacOS下gnu命令行

#### 安装

```ruby
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
上面这句话的意思是使用curl命令行下载安装脚步install，然后用ruby解析执行这个安装脚步。

安装好之后

```
echo 'export PATH="/usr/local/bin:$PATH"' >> ~/.bash_profile
```
后面的`.bash_profile`请根据你使用shell的情况来修改。

现在homebrew的最新版本是1.0.0。

使用以下命令查看版本。

```
brew -v
```

#### 基本操作

###### 安装一个包

```
brew install <formula>
```
但是麻烦的是你往往不记得包名或者记不全，这里有两个方法，第一个是使用`brew search`进行搜索，比如你想找vim这个包，但你只记得了前面两个字母，最后一个 **m** 给忘记了，这时候你可以尝试下面但命令：

```
brew search vi
```
结果出来了，但是结果太多了，包括了很多不是 **vi** 开头的包，这个时候你就得用正则表达式来搜索，这对小白来说也是非常痛苦的，因为要学会正则表达式也得看一本书，但是一旦你熟悉它之后，你会发现你会在每个搜索的场景下都希望它支持正则表达式的搜索，正如你熟悉了编辑器vim之后，希望处处都是vim模式一样。

```
brew search /正则表达式/ # 标准格式
brew search /^vi/   #规定了只能是vi开头
brew search /^vi\\w$/   #规定只能是vi开头并且只有三个字母
```
第二种方法更适合小白用户，前提是你安装了oh-my-zsh，使用过[oh-my-zsh][oh-my-zsh]的用户都知道其补全功能非常牛逼，它可以补全命令，命令的选项和参数，还可以补全包管理器的包名，是不是很厉害，但是默认oh-my-zsh是不支持HomeBrew的，所以我们需要这样做：

```
brew install zsh-completions
```
使用homebrew安装原本需要图形安装的软件比如chrome。

```
brew cask install <formula>
```
一般来说不带任何选项的话，homebrew会优先下载二进制，二进制下载不到就会尝试从源码编译，这也是homebrew强大的地方之处。

比如，我们希望更新最新的vim，这就得从源码编译了，从源码编译过vim的人都知道，有很多选项要用户决定，然后homebrew是如何做到的呢，我们该如何指定选项呢？

```
brew info <formula>  #查看这个包的信息，从中我们可以得知有哪些选项可选。

#示例
brew install vim --HEAD --with-override-system-vi --with-lua
```
然后，homebrew就会帮我们解决编译过程中的任何依赖了，是不是很爽？

细心的人可能也注意到了，并没有找到所有在vim源码makefile中的提供的选项，这是因为支持什么选项取决于该包对应的 **formula** 文件，通常位于`/usr/local/Homebrew/Library/Taps/homebrew/homebrew-core/Formula`文件夹中，你可以使用下面的命令来编辑它，该文件遵循ruby语法：

```
brew edit <package_name>
```

#### 卸载与跟新

```
# 卸载对应包名字
brew uninstall <package_name>
# 列出过时的包
brew outdated
# 更新过时的包，不带包名就更新所有包
brew upgrade [ package_name ]
# 更新HomeBrew自身
brew update
# 清除缓存
brew cleanup [包名]
# 列出已经安装的包
brew list
```

如果该包有多个版本，那么先使用`brew switch <包名> <版本号>`来切换到该版本然后再使用`uninstall`来卸载，如果卸载全部版本那么使用`--force`选项。

#### 扩展

就像ubuntu的ppa一样，很多时候有些软件包并不在官方提供列表里面而是由第三方提供的这个时候，我们就需要使用下面的命令：

```
brew [un]tap <github_userid/repo_name> #添加或者删除仓库
```
注意`repo_name`只是实际仓库名的一部分，而实际仓库名的前缀必须是`homebrew-`。比如

```
brew tap neovim/neovim
# 这样实际仓库名就是homebrew-neovim
```
[官方文档][brewhome]中提出brew tap作用用于添加更多仓库，默认情况下tap假设这些仓库来源于[github][github]，但又不局限于它。

不带参数的话，将会列出当前已经`tapped`的仓库，比如：

```
brew tap
==> Auto-updated Homebrew!
Updated 1 tap (homebrew/core).
No changes to formulae.

caskroom/cask
homebrew/core
homebrew/dupes
neovim/neovim
```
总共列出了四个仓库，其中前面三个是默认自带的。

如果你要增加的仓库已经存在于`homebrew/core`中了(名字一样)，你必须显性的安装：

```
brew install vim                     # installs from homebrew/core
brew install username/repo/vim       # installs from your custom repo
```

#### 更换源

由于各种原因，用homebrew跟新下载软件有时非常慢，这个时候你可以尝试更换源，这个概念和其它包管理器的概念是一致的，也就是换个软件服务器。

有两个源，第一个是homebrew自身程序公式的服务器地址，homebrew是托管于github，如果你访问这个网站没有问题，那就不需要换了，要换也非常简单，相当于给你的git仓库换一个远程地址，而homebrew的仓库位置默认位于/usr/local/Homebrew下（这个位置是homebrew 1.0之后才变的）。

```
cd /usr/local/Homebrew
git remote set-url origin http://mirrors.ustc.edu.cn/homebrew.git

cd ~
mkdir tmp
cd tmp
git clone http://mirrors.ustc.edu.cn/homebrew.git

cp -R homebrew/.git /usr/local/Homebrew
cp -R homebrew/Library /usr/local/Homebrew
```

第二个源就是二进制的服务器地址，做法很简单就是

```
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.zshrc
source ~/.zshrc
```

#### Homebrew中一些路径的解释

Homebrew 1.0的基础上，

```
Caskroom  Frameworks  bin  include  opt   share
Cellar    Homebrew    etc  lib      sbin  var
```

* `Cellar`:文件夹存放的是所有包安装所在路径，包括二进制，文档和配置文件，按照这样Cellar/包名/版本号/的形式来安放。

* `opt`:由于版本号随着跟新而改变的，所以需要一个固定不变的路径作为我们访问二进制和文档的路径，这就是opt的作用。

* `Homebrew`:brew程序所在路径.

* `bin`:所有包安装之后二进制都会链接到这个路径下

* `share`:所有包安装之后的文档都会链接到这个路径下

* `etc`:同上，所有包的配置文件

* `lib`:同上，所有包相关库文件

* `Caskroom`:app文件

#### MacOS下gnu命令行

MacOS下的命令行是bsd的，而且好久没跟新，各种不顺手，所以这一小节的目的是介绍通过homebrew安装gnu的命令行工具代替系统自带的命令行。

```
# GNU File, Shell, and Text utilities
brew info coreutils
brew install coreutils
```
从第一条命令得知，安装之后，所有命令都是带有g前缀的，这会让我们非常不爽，所以如果你希望使用它们原来的名字的话，就将这个路径加到PATH变量的最前头。

```
PATH="/usr/local/opt/coreutils/libexec/gnubin:$PATH" #这句话放到你shell配置文件中
MANPATH="/usr/local/opt/coreutils/libexec/gnuman:$MANPATH" #默认使用他们的manpage
```
可以看出还是比较麻烦的，而且有些包像binutils并没有提供这样的链接，但是却有g前缀。

下面这些软件也是我们常用的，

```
brew install binutils
brew install ed --with-default-names
brew install findutils --with-default-names
brew install gawk
brew install curl --with-libidn --with-libssh2 --with-nghttp2 --with-rtmpdump
brew install gnu-indent --with-default-names
brew install gnu-sed --with-default-names
brew install gnu-tar --with-default-names
brew install gnu-which --with-default-names
brew install gnutls
brew install grep --with-default-names
brew install gzip
brew install screen
brew install watch
brew install wdiff --with-gettext
brew install wget
brew install bash zsh
brew install gdb  # gdb requires further actions to make it work. See `brew info gdb`.
brew install gpatch
brew install m4
brew install make
brew install nano
brew install rsync
brew install svn
brew install unzip
brew install aria2
brew install git 
brew install ffmpeg
brew install ctags cscope the_silver_searcher
brew install vim --HEAD --with-override-system-vi --with-lua
brew install neovim --HEAD --with-release
```

[brew]:https://brew.sh/index_zh-cn.html
[oh-my-zsh]:https://github.com/robbyrussell/oh-my-zsh
[brewhome]:https://github.com/Homebrew/brew/blob/master/docs/brew-tap.md
[github]:https://github.com