# 5 Newt管理器指南

Newt Manager（*newtmgr*）是一个供用户与远程运行Mynewt OS的设备通信与管理的应用工具。它使用一个连接配置文件与远程设备建立连接，并向设备发送命令请求。此工具的命令结构与newt工具类似。

## 5.1 命令列表

以下为newtmgr命令，具体可通过-h标识来获取每个命令的详细信息。

```
Available Commands:
  config      Read or write a config value on a device
  conn        Manage newtmgr connection profiles
  crash       Send a crash command to a device
  datetime    Manage datetime on a device
  echo        Send data to a device and display the echoed back data
  fs          Access files on a device
  help        Help about any command
  image       Manage images on a device
  log         Manage logs on a device
  mpstat      Read mempool statistics from a device
  reset       Perform a soft reset of a device
  run         Run test procedures on a device
  stat        Read statistics from a device
  taskstat    Read task statistics from a device

Flags:
  -c, --conn string       connection profile to use
  -h, --help              help for newtmgr
  -l, --loglevel string   log level to use (default "info")
      --name string       name of target BLE device; overrides profile setting
  -t, --timeout float     timeout in seconds (partial seconds allowed) (default 10)
  -r, --tries int         total number of tries in case of timeout (default 1)
```

- newtmgr config：读写设备上的配置值，通过conn_profile连接配置文件连接到远程设备

  ```
  newtmgr config <var-name> [var-value] -c <conn_profile> [flags]
  -c, --conn string       connection profile to use
  -h, --help              help for newtmgr
  -l, --loglevel string   log level to use (default "info")
      --name string       name of target BLE device; overrides profile setting
  -t, --timeout float     timeout in seconds (partial seconds allowed) (default 10)
  -r, --tries int         total number of tries in case of timeout (default 1)
  ```

- newtmgr conn：管理newtmgr连接配置文件

```
newtmgr conn [command] [flags]
-c, --conn string       connection profile to use
-l, --loglevel string   log level to use (default "info")
    --name string       name of target BLE device; overrides profile setting
-t, --timeout float     timeout in seconds (partial seconds allowed) (default 10)
-r, --tries int         total number of tries in case of timeout (default 1)
```

conn命令提供用于添加、删除和查看连接配置文件的子命令。连接配置文件指定如何与远程设备进行连接和通信的信息。newtmgr命令使用连接配置文件的信息，发送请求到远程设备。

**关于newtmgr子命令**

Add命令：创建一个conn_profile连接配置文件，指定一系列变量，变量间通过空格区分。

```
newtmgr conn add <conn_profile> <var-name=value ...>
```

var-name包括type和connstring，有效的值有：

1. type：连接类型

   - serial：newtmgr协议通过串行连接
   - oic_serial：OIC协议通过串行连接
   - udp：newtmgr协议通过UDP
   - oic_udp：OIC协议通过UDP
   - ble：newtmgr协议通过BLE，主机操作系统需要支持BLE
   - oic_ble：OIC协议通过BLE，主机操作系统需要支持BLE
   - bhd：newtmgr协议通过BLE，使用blehostd实现
   - oic_bhd：OIC协议通过BLE，使用blehostd实现

   注意：newtmgr在windows上不支持BLE

2. connstring：物理或用于连接的虚拟地址

   - serial和oic_serial
   - udp和oic_udp
   - ble和oic_ble
   - bhd和oic_bhd

- newtmgr crash
- newtmgr datetime
- newtmgr echo
- newtmgr fs
- newtmgr image
- newtmgr log
- newtmgr mpstat
- newtmgr reset
- newtmgr run
- newtmgr stat
- newtmgr taskstat

## 5.2 *newtmgr*安装

