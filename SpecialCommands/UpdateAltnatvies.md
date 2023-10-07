# update-alternatives on Ubuntu

使用这个命令是在尝试对Ubuntu 20.04自带的Python 3.8升级时需要使用到的。强烈不建议升级Ubuntu自带的Python版本。因为很多Ubuntu 20.04系统自带的组件是于自带的Python 3.8强依赖。一旦修改相关环境变量或者升级Python本身会出现很多意外情况。

## reference

1. 这个参考已经非常详细了<https://www.jianshu.com/p/4d27fa2dce86>

## relevant parameters

update-alternatives 命令用于处理 Linux 系统中软件版本的切换，使其多版本共存。alternatives 的管理目录 /etc/alternatives 。

### alternatives管理方式

```shell
$ ls -l /usr/bin/python
lrwxrwxrwx 1 root root 24 1120  2017 /usr/bin/python -> /etc/alternatives/python

$ ls -l /etc/alternatives/python
lrwxrwxrwx 1 root root 18 1121  2017 /etc/alternatives/python -> /usr/bin/python3.8
```

python 这个可执行命令实际是一个链接，指向了 /etc/alternatives/python 。而这个也是一个链接，指向了 /usr/bin/python3.8 ，这才是最终的可执行文件。alternatives 实际上是通过软链接的方式对版本进行管理。

### 语法

```shell
$ update-alternatives --help
用法：update-alternatives [<选项> ...] <命令>

命令：
  --install <链接> <名称> <路径> <优先级>
    [--slave <链接> <名称> <路径>] ...
                           在系统中加入一组候选项。
  --remove <名称> <路径>   从 <名称> 替换组中去除 <路径> 项。
  --remove-all <名称>      从替换系统中删除 <名称> 替换组。
  --auto <名称>            将 <名称> 的主链接切换到自动模式。
  --display <名称>         显示关于 <名称> 替换组的信息。
  --query <名称>           机器可读版的 --display <名称>.
  --list <名称>            列出 <名称> 替换组中所有的可用候选项。
  --get-selections         列出主要候选项名称以及它们的状态。
  --set-selections         从标准输入中读入候选项的状态。
  --config <名称>          列出 <名称> 替换组中的可选项，并就使用其中哪一个，征询用户的意见。
  --set <名称> <路径>      将 <路径> 设置为 <名称> 的候选项。
  --all                    对所有可选项一一调用 --config 命令。

<链接> 是指向 /etc/alternatives/<名称> 的符号链接。(如 /usr/bin/pager)
<名称> 是该链接替换组的主控名。(如 pager)
<路径> 是候选项目标文件的位置。(如 /usr/bin/less)
<优先级> 是一个整数，在自动模式下，这个数字越高的选项，其优先级也就越高。
```

### Examples

1. display参数显示关于python替换组的信息
   ```shell
   $ update-alternatives --display python 
    python - 手动模式
    link best version is /usr/bin/python3.10
    链接目前指向 /usr/bin/python3.8
    link python is /usr/bin/python
    /usr/bin/python3.8 - 优先级 1
    /usr/bin/python3.10 - 优先级 2
   ```
2. 选择候选项
   ```shell
   $ update-alternatives --config python    
    有 2 个候选项可用于替换 python (提供 /usr/bin/python)。
    选择       路径              优先级  状态
    ------------------------------------------------------------
    0            /usr/bin/python3.10   2         自动模式
    * 1            /usr/bin/python3.8   1         手动模式
    2            /usr/bin/python3.10   2         手动模式

    要维持当前值[*]请按<回车键>，或者键入选择的编号：
   ```
3. install参数用于添加一个命令的link值。
   ```shell
    # 添加 python link
    $ update-alternatives --install /usr/bin/python python /usr/bin/python3.8 2

    # 第一个参数: --install 表示向update-alternatives注册服务名。
    # 第二个参数: 注册最终地址，成功后将会把命令在这个固定的目的地址做真实命令的软链，以后管理就是管理这个软链；
    # 第三个参数: 服务名，以后管理时以它为关联依据。
    # 第四个参数: 被管理的命令绝对路径。
    # 第五个参数: 优先级，数字越大优先级越高。
   ```
4. remove参数用于删除一个命令的link值，其附带的slave也将一起删除。
   ```shell
   $ update-alternatives --remove python /usr/bin/python3.8
   ```
   移除名字的相关所有链接:
   ```shell
   $ update-alternatives --remove-all python
   ```
