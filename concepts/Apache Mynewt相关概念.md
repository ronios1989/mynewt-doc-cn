# Apache Mynewt相关概念

## 项目（Project）

项目是嵌入式软件树的基础目录。是一个包含源代码逻辑集合的工作空间，实现一个或多个应用。一个项目包含以下内容：

- 项目定义：定义项目级别依赖关系、参数（位于*project.yml*）
- 包

默认Apache Mynewt项目的定义如下：

```
# project.yml
# <snip>
project.name: "my_project"

project.repositories:
    - apache-mynewt-core

# Use github's distribution mechanism for core ASF libraries.
# This provides mirroring automatically for us.
#
repository.apache-mynewt-core:
    type: github
    vers: 1-latest
    user: apache
    repo: mynewt-core
```

其中：

- project.repositories: 定义项目所依赖的远程仓库
- repository.apache-mynewt-core: 定义apache-mynewt-core仓库的信息
- vers=1-latest: 定义仓库的版本。latest将使用github上Master分支的最新稳定版本。要使用master分支的最新版本，需要将此参数改为vers=0-dev，但是需要注意此分支可能并不稳定

存储仓库是包的不同版本的集合。

项目可以依赖远程存储库来实现功能，而newt工具可以解析这些远程仓库，并将正确的版本下载到本地的源码树中。最新获取的仓库将被放置到项目的repos中，并且可以通过使用@specifier在系统中引用。

默认情况下，@apache-mynewt-core仓库将会被每个项目包含。Apache Mynewt Core包含Apache Mynewt操作系统的基本功能，包含实时内核，蓝牙网络协议栈，Flash文件系统，控制台，Shell工具以及Bootloader。

**注意**：任何项目通过提供一个*repository.yml*文件而转化到一个仓库，并将其推到Github。关于更多数据仓库系统信息可以在Newt文档中查看。

## 包（package）

包是组成Mynewt操作系统功能单元的元素集合，包可以包含：

- 应用（Applications）
- 库文件（Libraries）
- 编译器定义（Compiler definitions）
- 目标（Targets）

在每个包的根目录下包含一个*pkg.yml*文件，下面是示例应用blinky的*pkg.yml*文件：

```
# pkg.yml
# <snip>
pkg.name: apps/blinky
pkg.type: app
pkg.description: Basic example application which blinks an LED.
pkg.author: "Apache Mynewt <dev@mynewt.apache.org>"
pkg.homepage: "http://mynewt.apache.org/"
pkg.keywords:

pkg.deps:
    - "@apache-mynewt-core/libs/os"
    - "@apache-mynewt-core/hw/hal"
    - "@apache-mynewt-core/libs/console/full"
```

包中以下特性需要注意：

- 依赖：包可以依赖其他的包，定义依赖后，将可以继承它们的功能（头文件、库定义等）
- APIs：包可以导出应用编程接口，且要求某些特定的API存在，以便能够编译

Newt对于项目目录的了解全依赖于一个包，这使能编写一个可重用的组件变得非常干净与轻松，这可以将他们的依赖关系以及API对系统的其他部分描述。

## 目标（Target）

Apache Mynewt的Target与make中的目标非常类似。它是必须传递给Newt的参数的集合，从而Newt可以生成一个可复制的构建。目标代表构建树的顶层，在目标级别指定的任何包或参数，都会级联到所有依赖项。

目标也是包，存放在项目根目录的*targets/*目录下，大多数的目标都包含：

- app：构建的应用
- bsp：应用程序相结合的板级支持包
- build_profile：*debug*或*optimized*

目标还可以指定一些额外的项目，包括：

- aflags：希望在编译过程中指定的特定的汇编器标志
- cflags：希望在编译过程中指定的特定的编译器标志
- lflags：希望在编译过程中指定的特定的链接器标志

为了创建和操作目标，Newt工具提供了一组帮助指令，可以从中找到更多有用信息：

```
$ newt target
Usage:
  newt target [flags]
  newt target [command]

Available Commands:
  amend       Add, change, or delete values for multi-value target variables
  cmake
  config      View or populate a target's system configuration
  copy        Copy target
  create      Create a target
  delete      Delete target
  dep         View target's dependency graph
  revdep      View target's reverse-dependency graph
  set         Set target configuration variable
  show        View target configuration variables

Global Flags:
  -h, --help              Help for newt commands
  -j, --jobs int          Number of concurrent build jobs (default 2)
  -l, --loglevel string   Log level (default "WARN")
  -o, --outfile string    Filename to tee output to
  -q, --quiet             Be quiet; only display error output
  -s, --silent            Be silent; don't output anything
  -v, --verbose           Enable verbose output when executing commands

Use "newt target [command] --help" for more information about a command.
```

## 配置（Configuration）

额外的一些帮助话题：

```
$ newt target config show <target-name>
...
* PACKAGE: sys/stats
  * Setting: STATS_CLI
    * Description: Expose the "stat" shell command.
    * Value: 0
  * Setting: STATS_NAMES
    * Description: Include and report the textual name of each statistic.
    * Value: 0
  * Setting: STATS_NEWTMGR
    * Description: Expose the "stat" newtmgr command.
    * Value: 0
```

