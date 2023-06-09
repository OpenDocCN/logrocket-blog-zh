# 如何建立一个用于网络开发的 M1 MacBook-log rocket 博客

> 原文：<https://blog.logrocket.com/how-to-set-up-m1-macbook-web-development/>

***编者按**:这篇文章最后一次更新是在 2022 年 3 月 29 日，以反映对家酿、Docker 桌面和 VS 代码的更新。*

自从几天前我第一次拿到我的 M1 MacBook Air，我就一直在挑战它的极限。这台机器不仅速度快，我还安装了多个并行运行的设备，温度几乎没有达到 104 华氏度的峰值。总的来说，与英特尔芯片型号相比，这台机器处于一个全新的水平。

不幸的是，我花了很长时间来为理想的网络开发环境设置我的 MacBook，因为我很难找到概述我正在寻找的所有信息的资源。

我整理了一个教程，可以帮助你在 20 分钟内安装并运行你的 web 开发工具。这个 web 开发环境包括以下内容:

*   罗塞塔 2 号
*   公司自产自用
*   饭桶
*   节点. js
*   MongoDB
*   谷歌浏览器
*   火狐浏览器
*   VS 代码
*   npm(消歧义)
*   nvm(消歧义)
*   你好
*   哦，我的 Zsh
*   电力线字体

我们开始吧！

## 目录

## 我升级 MacBook Pro 的原因

经过 1200 多次充电后，我用了六年的 13 英寸 MacBook Pro 变得越来越不可靠。就在苹果发布 M1 产品线的时候，我面临着购买新电脑的艰难选择。

对新芯片的每一次评论都描绘了一幅不可思议的画面，它们都有一个共同点；芯片速度非常快。不过，考虑到 M1 是基于 ARM 架构的，这个决定并不那么简单。

苹果过渡到自己的芯片已经有几年了。苹果曾声称将在未来几年取代基于英特尔的芯片。但是，在撰写本文时，唯一一款尚未过渡到苹果芯片的 Mac 产品是 Mac Pro，即台式 Mac，它可能于 2022 年底与苹果芯片一起推出。

app store 中的大部分应用已经过渡到了苹果芯片，甚至是开发者工具。在本文中，我将向您展示如何在您的 M1 Mac 上设置这些开发工具。你需要遵循教程的所有东西都是预装在你的 Mac 上的，所以我们会花大部分时间在终端上安装列表上的工具。我们开始吧！

## 罗塞塔 2 号

首先，我们需要设计在英特尔上运行的软件，以便与我们新制造的处理器使用相同的语言。苹果推出了一个无缝过渡的解决方案，一个名为 [Rosetta 2](https://developer.apple.com/documentation/apple_silicon/about_the_rosetta_translation_environment) 的仿真器，允许我们在 ARM 上运行使用 x86 指令的应用程序，x86 指令是英特尔芯片使用的指令集。

苹果计划在向苹果芯片的过渡完成后移除 Rosetta 2。但在那之前，大多数工具也应该过渡到苹果芯片。

请记住，默认情况下不会安装 Rosetta。要使用它，我们必须进入**实用程序**文件夹中预先安装的**终端**并运行以下命令:

```
/usr/sbin/softwareupdate --install-rosetta --agree-to-license

```

`--agree-to-license`标志会跳过交互安装，同意苹果的许可。但是，如果您想调查您注册的目的，请随意删除该标志并阅读许可证。

另一种安装 Rosetta 2 的方法是简单地打开一个基于 Intel 的应用程序，然后会提示您安装 Rosetta 2:

![Install Rosetta Intel](img/6b79535e10cf9c4eab917b5a980c8c80.png)

## 公司自产自用

我们将使用[自制软件](https://brew.sh/)来安装我们的大多数应用程序和实用程序。当 2020 年 11 月首次推出 M1 MAC 电脑时，家酿啤酒没有得到适当的支持。在更新这篇文章的时候，M1 Macs 完全支持 Homebrew，没有任何问题。

打开您的**终端**，运行以下命令，并在出现提示时输入您计算机的密码:

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

```

家酿完成安装后，我们可以用它来安装我们的大部分工具。我有一个为我做繁重工作的脚本，它来自于 zellwk 的 GitHub repo，并根据我的环境进行了调整。你可以在 GitHub 回购之后的[中找到我的脚本版本。](https://github.com/CxGarcia/setup)

去后面的回购下载吧。解压后，`cd`进入`setup`文件夹。回购包括我的一些配置文件，但我们将只使用本教程的`brew-installs.sh`脚本。

在继续之前，我建议您在文本编辑器中打开`brew-installs.sh`,查看它所做和安装的一切。您可以调整它以适应您的环境，因为我不希望您使用和我一样的工具。

一旦你对它感到满意，通过运行`ls -al`来检查`brew-installs.sh`文件是否可执行。如果文件是不可执行的，输出将类似于`-rw-r-xr-x ... brew-installs.sh`。三个点代表一些与我们的目的无关的信息。

要使它可执行，只需运行`chmod +x brew-installs.sh`。这个命令现在应该输出`-rwxr-xr-x ... brew-installs.sh`。

现在，假设您当前的工作目录已经设置好，通过在您的终端中输入`./brew-installs.sh`来运行`brew-installs.sh`脚本。在这里，您可以让脚本为您施展魔法。根据你的网速，下载和安装所有东西大概需要五分钟。

## iTerm

先前安装脚本中包含的 iTerm 现在应该已经安装好了，因此，我们可以用它来完成本教程。像我们第一次在终端上做的那样添加 Rosetta 层是很重要的。一种选择是复制应用程序并创建一个 Rosetta iTerm 和一个常规 iTerm。我们可以通过下面的 GIF 做到这一点:

![Set up M1 macbook homebrew](img/90c9685b4b820b9e56632f2171a4c981.png)

## Zsh

如果您没有从`brew-installs.sh`脚本中排除 [Zsh](https://github.com/zsh-users/zsh) ，那么它现在应该是您的默认 shell 了。如果您确实排除了它，运行`brew install zsh`。在我们继续之前，让我们通过运行`echo $SHELL`来确保 Zsh 是默认的 shell，它应该会输出`/bin/zsh`。如果没有，通过运行`chsh -s $(which zsh)`切换到 Zsh。

## Oh My Zsh

让我们用[哦，我的 Zsh](https://ohmyz.sh/) 给 Zsh 一些额外的活力。通过运行以下命令来安装它:

```
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

```

Oh My Zsh 有很多插件和主题。您可以在 [GitHub repo](https://github.com/ohmyzsh/ohmyzsh/wiki/Plugins) 中查看完整列表。语法高亮是我离不开的插件之一。

哦，我的 Zsh 使得识别你是否输入了正确的命令或者它是否在你的路径上变得更加容易。如果命令被识别，文本将为绿色。如果没有，它将是红色的:

![Oh My Zsh Command Recognized](img/1ec5aafaabdaf1110a2a574350dd2145.png)

为了减少混乱，最好首先将`cd`放入`cd $HOME/.oh-my-zsh/plugins`路径来安装插件，然后运行下面的命令，它会自动将文件夹的源代码添加到您的`.zshrc`文件中:

```
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
echo "source ${(q-)PWD}/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc

```

## nvm

我试图通过自制软件安装 nvm，但是失败了，因为旧版本的 Node.js 不被 M1 架构支持。所以，你需要用 Rosetta 2 来安装 nvm，我们之前已经安装好了。最好的替代方法是通过运行以下命令[通过 cURL](https://blog.logrocket.com/an-intro-to-curl-the-basics-of-the-transfer-tool/) 安装 nvm:

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash

```

安装完成后，将下面几行添加到您的`.zshrc`文件中。如果您正在使用 Bash，您必须将它添加到您的主目录中的`.bash-profile`或`.bashrc`:

```
export NVM_DIR="$HOME/.nvm"
#This loads nvm
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
#This loads nvm bash_completion
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"

```

重置您的控制台会话，并通过运行`nvm --version`检查 nvm 是否安装正确，它应该会输出`your current version 0.37.2`。

## Git 和 GitHub

[Git](https://git-scm.com/) 是 brew-installs 中包含的安装之一，所以现在应该已经安装了。为了[配置 Git](https://blog.logrocket.com/git-strategies-collaborate-share-maintain-working-history/) ，让我们首先设置我们的用户名和电子邮件。

如果您使用 XCode，或者您安装了 XCode 命令行工具，那么 Git 应该已经安装在您的机器上了。Apple 安装 XCode 运行所需的所有软件。

用您自己的命令替换`< USERNAME >`和`< EMAIL >`,并运行以下命令序列:

```
git config --global user.name < USERNAME > &&
git config --global user.email < EMAIL > &&
git config --global --list

```

推荐的认证 GitHub 的方法是通过个人访问令牌。不过这已经超出了本教程的范围，所以请访问 [官方 GitHub 教程](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token) 。

## VS 代码

为了实现从一台计算机到另一台计算机的无缝转换，VS Code 包含了一个名为 [Settings Sync](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync) 的扩展，它允许你将你的配置上传到 GitHub Gist。一旦它们在 GitHub 上运行，扩展就会负责保持以下文件的同步:设置、按键绑定、代码片段、工作区文件夹、扩展及其相应的配置。

扩展页面对如何设置 VS 代码有全面的解释。用您喜欢的设置设置 VS 代码应该只需要几分钟。

在撰写本文时， [Visual Studio 代码已经过渡到苹果芯片](https://code.visualstudio.com/download)。所以，如果你还在使用基于英特尔的 VS 代码版本，苹果芯片版本应该会给你一个巨大的性能提升。

## 电力线字体

Oh My Zsh 中的大多数主题都需要[电力线字体](https://github.com/powerline/fonts)。为了方便起见，我从官方电力线报告中提取了以下信息，并将命令转换成一个序列。将下面的代码块复制并粘贴到终端中，它将为您下载并安装电力线字体:

```
git clone https://github.com/powerline/fonts.git --depth=1 &&
cd fonts &&
./install.sh &&
cd ..

```

您现在可以删除通过运行`rm -rf fonts`创建的 fonts 文件夹。出于安全考虑，我没有将这个命令放在序列中。

如果您正在使用 Agnoster 或任何其他使用电力线的主题，出于某种原因，您会看到问号而不是图标，您必须在 iTerm 设置中更改非 ASCII 字体。你可以在**档案**的**文本**标签中找到。我个人使用[空间单声道用于电力线](https://github.com/powerline/fonts/tree/master/SpaceMono)，但还有许多其他选项可供您查看。

## 结论

现在，你的 M1 MacBook 已经可以进行网络开发了！虽然这篇文章可能很密集，但好消息是，一旦用 npm 安装了所需的`node_modules`,您的项目就应该可以编译了。

如果你有任何问题或担心，请留下评论，我很乐意帮助你。我希望你喜欢这篇文章。编码快乐！

## 使用 [LogRocket](https://lp.logrocket.com/blg/signup) 消除传统错误报告的干扰

[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/signup)

[LogRocket](https://lp.logrocket.com/blg/signup) 是一个数字体验分析解决方案，它可以保护您免受数百个假阳性错误警报的影响，只针对几个真正重要的项目。LogRocket 会告诉您应用程序中实际影响用户的最具影响力的 bug 和 UX 问题。

然后，使用具有深层技术遥测的会话重放来确切地查看用户看到了什么以及是什么导致了问题，就像你在他们身后看一样。

LogRocket 自动聚合客户端错误、JS 异常、前端性能指标和用户交互。然后 LogRocket 使用机器学习来告诉你哪些问题正在影响大多数用户，并提供你需要修复它的上下文。

关注重要的 bug—[今天就试试 LogRocket】。](https://lp.logrocket.com/blg/signup-issue-free)