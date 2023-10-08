# Git 的安装和使用

**Git的版本发布时间线**





## 1.1 节  创建第一个仓库，然后添加并提交文件

使用命令行去验证Git是否安装，

在所有操作系统使用命令：

```shell
git --version
```

在类Unix操作系统：

```shell
whitch git	
```

如果没有任何返回，或这个命令没有被识别，在你的操作系统下载并运行安装器来安装Git。具体的操作可以查看[Git 官网](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)来获取更清晰、简洁的安装介绍。

安装Git后，配置 `username` 和 `email address`。 这一步需要在在提交之前完成。

当Git安装后，移动到你想要进行版本控制的文件夹下面，创建一个空仓库：

```shell
git init
```

这将会创建一个包含Git所需要的管道的隐藏的文件夹`.git`。

接下来，检查Git将向新仓库当中添加哪些文件，**这一步值得特别注意：**

```shell
git status
```

检查生成的文件列表，告诉Git哪些文件将置于版本控制之下（避免添加带有机密信息（例如密码）的文件，或者会使仓库混乱的文件）：

```shell
git add <file/directory name #1>  <file/directory name #2> <...>
```

或者可以使用多次 `add` 命令添加文件

```shell
git add <file/directory name #1>
git add <file/directory name #2>
git add <file/directory name #3>
git add <...>
```

如果列表中的所有文件都应该共享给仓库的使用者，那么只需要一个命令就可以添加当前目录和子目录的所有内容：

```shell
git add .
```

在这个阶段所有文件被添加到版本控制当中，准备将它们在你的首次提交中进行提交。

对于那些你永远都不想置于版本控制的文件，在执行 `add` 命令之前，创建并把需要忽略的文件的文件名写入 `.gitignore` 文件。

提交之前添加的所有文件，并附加提交信息：

```shell
git commit -m "Initial commit"
```

这将使用给定信息创建一个新的提交，提交就像对项目进行保存或者快照。现在可以进行推送或者上传到远程仓库，如果需要也可以回退上一步。

如果省略 `-m` 参数，会使用默认的文本编辑器会被打开，在这里你可以编辑和保存提交信息。

要添加新的远程，在仓库所在的目录上用终端使用 `git remote` 命令。

`git remote` 命令有两个参数

1. 远程仓库的名称， 例如 *origin*
2. 远程仓库的URL， 例如 https://<yout-git-service-address>/user.repo.git

```shell
git remote add orign https://<yout-git-service-address>/user.repo.git
```

> 注意：在添加远程远程仓库之前你需要确保在远程服务器上存在该仓库，如此操作你才能执行 `push/pull` 命令。



## 1.2 节 Clone 仓库

`git clone` 命令常常用来从远程服务器复制已经存在的Git仓库到本地。

例如，复制一个Github项目：

```shell
cd /path/you/need/clone/
git clone https://github.com/username/projectname.git
```

复制一个Bitbucket项目

```
cd /path/you/need/clone/
git clone https://youtusername@bitbucket.org/username/projectname.git
```

在本地机器创建一个叫做projectname的文件夹，并将远程Git仓库的所有文件复制过来。包括源文本还有包括该项目所有提交历史和配置信息的`.git` 文件夹

若需要一个不与远程同名的文件夹，例如复制到 `MyFolder` 目录

```shell
git clone https://github.com/username/projectname.git MyFolder
```

或者复制到当前目录下

```shell
git clone https://github.com/username/projectname.git .
```

注意：

1. 需要clone到具体的文件夹，这个文件夹必须是空的或者不存在的
2. 同样可以使用 `ssh` 模式

```
git clone git@github.com:username/projectname.git
```

使用`https`和使用`ssh`模式相同，但是有些服务商，Github推荐使用`https`而不是`ssh`。



## 1.3 节 共享代码

要共享代码，您需要在远程服务器上创建一个仓库，将本地仓库复制到该远程仓库。

为了最小化远程服务器上的空间使用，你创建一个只有`.git`空仓库，并且不在文件系统中创建工作副本的仓库。你可以将此远程设置为上游服务器，以便轻松的与其他程序员共享更新。

在远程服务器：

```
git init --bare /path/to/repo.git
```

在本地机器上：

```shell
git remote add origin ssh://username@server/path/to/repo.git
```

> `ssh` 只是其中一种访问远程仓库的方式

现在可以复制到你的本地仓库到远程：

```
git push --set-upstream origin master
```



## 1.4 节 设置你的用户名和邮箱

需要在创建任何提交之前设置您的身份。这将允许提交记录拥有与其关联的作者姓名和电子邮件。

**值得注意的是，这与推送到远程仓库时的身份验证无关（例如，当使用 GitHub、BitBucket 或 GitLab 帐户推送到远程仓库时）**

要对所有的仓库设置用户身份，可以用`git config --global`，将配置信息存储在用户的`.gitconfig`文件当中。

目录在 `$HOME/.gitconfig`或者对于Windows，在`%USERPROFILE%\.gitconfig`

```
git config --global user.name "Your Name"
git config --global user.email mail@example.com
```

上面讲述了针对所有仓库的设置，如果针对单个仓库声明身份，可以在对应的仓库里使用 `git config` 命令。

这会将设置存储在单个仓库内的文件 `$GIT_DIR/config` 中。 例如
在`/path/to/your/repo/.git/config`中，可以按照如下方式进行设置。

```
cd /path/to/my/repo
git config user.name "Your Login At Work"
git config user.email mail_at_work@example.com
```

如果同时设置了全局配置和单个仓库配置，优先使用该仓库内的配置文件。

如果你包含多种不同的身份，例如用于开源项目的身份，用于工作的身份，用户私人仓库的身份等等，并且明确要分开使用不同的身份，那么可以如下操作：

- 移除全局身份认证

```
git config --global --remove-section user.name
git config --global --remove-section user.email
```

- 在`git --version ≥ 2.8`的时候，强制 git 仅在仓库内的设置中查找身份，而不是在全局配置中

```
git config --global user.useConfigOnly true
```

这样的话，如果你忘记为给定仓库设置 `user.name` 和 `user.email` 并尝试进行提交，会出现报错：

```
no name was given and auto-detection is disabled
no email was given and auto-detection is disabled
```



## 1.5 节 了解命令

要获取有关任何 git 命令的更多信息，即有关该命令的作用、可用选项和其他文档的详细信息，请使用 --help 选项或 `git help` 命令。

例如，要获取有关 `git diff` 命令的所有可用信息：

```
git diff --help
git help diff
```

同样，要获取有关 `status` 命令的所有可用信息：

```
git status --help
git help status
```

如果只想快速了解最常用的命令行标志的含义，请使用 -h