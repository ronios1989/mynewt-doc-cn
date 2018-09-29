# 4 Newt工具

## 4.1 Newt工具指南

Newt是一个针对嵌入式生产环境的智能构建及包管理系统。它是一个实现以下目标的单一工具：

- 源代码包管理
- 构建、调试和安装

### 基本原理

为了让Mynewt操作系统能够更好的适应不同类型的微控制器应用程序（从门铃到医疗设备，甚至到电网），通过newt工具来帮助你选择安装什么样的软件包和要构建的软件包。通过newt工具可以很方便的实现以下功能：

- 为多个目标分别构建
- 决定需要构建的包
- 管理组件之间的依赖关系

newt通过将常用的高级语言中的源包管理系统与make系统融合，很好的实现了Mynewt操作系统的源代码管理与目标构建、调试。

### 构建系统

一个好的构建系统必须允许用户通过采用一些常见的步骤来开发嵌入式应用，包括：

- 生成完整的Flash镜像
- 通过调试器将调试镜像下载到目标板
- 根据构建设置有条件地编译库和代码

Newt可以读取目录树，构建依赖树以及正确的构建结构。下例为mynewt-blinky/develop示例的源码树：

```
$ tree -L 3 .
├── DISCLAIMER
├── LICENSE
├── NOTICE
├── README.md
├── apps
│ └── blinky
│ ├── pkg.yml
│ └── src
├── project.yml
└── targets
    ├── my_blinky_sim
    │ ├── pkg.yml
    │ └── target.yml
    └── unittest
        ├── pkg.yml
        └── target.yml

6 directories, 10 files
```

当Newt识别到project.yml文件，它将会识别此目录为一个项目的根目录，并自动构建一个包结构。同时，它将识别包中的“apps”和“targets”。

**使用Newt工具创建Mynewt OS项目流程:**

1. Mynewt工程的创建：通过*newt newt <project-name>*创建项目；

2. 项目相关依赖解决：项目创建后，运行*newt install*下载、安装工程中所需要的依赖包；

3. 项目target的创建：关于目标的创建，则通过*$newt target create*指令创建

4. ```
   $newt target create target_name
   ```

   下载到目标板：

   ```
   $newt load target_name
   ```

完成项目的基本框架搭建后，需要使用Newt构建一个目标，在构建一个目标时，Newt会递归地解决所有的包依赖，并在bin/targets/<target-name>/app/apps/<app-name>目录中生成可执行文件，其中target-name为目标名称，app-name为应用程序的名字。

```
$newt target show   #查看当前项目中设置的目标情况
```

创建一个target之后，需要通过*newt target set*指令来设置target的app，bsp以及build_profile值。

其中app为要构建的应用，bsp为应用程序所配套的板级支持包，build_profile文件指明目标为debug或optimieze。这三个参数将存储在*target.yml*文件中。

在每一个包中都包含了一个*pkg.yml*文件，如：*apps/blinky/pkg.yml*文件：

```
$ more apps/blinky/pkg.yml
pkg.name: apps/blinky
pkg.type: app
pkg.description: Basic example application which blinks an LED.
pkg.author: "Apache Mynewt <dev@mynewt.apache.org>"
pkg.homepage: "http://mynewt.apache.org/"
pkg.keywords:

pkg.deps:
    - "@apache-mynewt-core/kernel/os"
    - "@apache-mynewt-core/hw/hal"
    - "@apache-mynewt-core/sys/console/stub"
```

通过pkg.yml文件，可以知道对应包的名称、类型（app，target）等信息，需要注意*pkg.deps*描述了项目所依赖的包*kernel/os*，*hw/hal*，*sys/console/stub*。

### Newt相关指令

一旦创建了一个目标，即可以通过Newt的一些操作对目标进行处理。

- load：下载生成的目标到目标板（该指令将通过下载脚本自动加载到目标板，若未与目标板连接将出现相关错误）

- debug：与目标之间打开一个调试器对话

- size：获取目标组件的大小（Flash大小以及RAM大小）

- ```
  $newt size <target-name>
  ```

  create-image：为二进制镜像增加标题、版本信息等

  ```
  $newt create-image <target-name> 1.0.0
  ```

- run： 构建、创建镜像、加载并最终打开带有目标的调试会话

- target：创建、删除、配置以及查询目标

- install：根据项目配置文件*project.yml*安装及下载相关依赖

- build：构建一个或多个目标

  ```
  newt build <target-name> [target-name ...] flags
  全局Flags包括：
  -h, --help              Help for newt commands
  -j, --jobs int          Number of concurrent build jobs (default 8)
  -l, --loglevel string   Log level (default "WARN")
  -o, --outfile string    Filename to tee output to
  -q, --quiet             Be quiet; only display error output
  -s, --silent            Be silent; don't output anything
  -v, --verbose           Enable verbose output when executing commands
  ```



## 4.2 Newt工具安装

### Mac OS系统安装



### Linux系统安装



### Windows系统安装



参考地址：

1、http://mynewt.apache.org/latest/newt/install/index.html