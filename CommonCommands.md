# Commond Commands

参考：[https://blog.csdn.net/zardforever123/article/details/125519727](https://blog.csdn.net/zardforever123/article/details/125519727)

| 编号 | 类别                      | 命令/语法      | 功能                                                                                                                                                                                                                                     | 举例                 | 参数 | 参数含义 | 说明                                                                                                                                                                                              |
| ---- | ------------------------- | -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------- | ---- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | Linux目录的创建与删除命令 | cd [directory] | 改变工作目录                                                                                                                                                                                                                             | cd ..                |      |          | 该命令将当前目录改变至directory所指定的目录。若没有指定directory， 则回到用户的主目录。为了改变到指定目录，用户必须拥有对指定目录的执行和读权限。该命令可以使用通配符（通配符含义请参见第十章）。 |
|      |                           | pwd            | 在Linux层次目录结构中，用户可以在被授权的任意目录下利用mkdir命令创建新目录，也可以利用cd命令从一个目录转换到另一个目录。然而，没有提示符来告知用 户目前处于哪一个目录中。要想知道当前所处的目录，可以使用pwd命令，该命令显示整个路径名。 | pwd                  |      |          | 此命令显示出当前工作目录的绝对路径。                                                                                                                                                              |
| 2    |                           | find           | 查找文件                                                                                                                                                                                                                                 | find path expression |      |          | find . -name 'srm*' #表示当前目录下查找文件名开头是字符串‘srm’的文件                                                                                                                            |
| 3    |                           |                |                                                                                                                                                                                                                                          |                      |      |          |                                                                                                                                                                                                   |
|      |                           |                |                                                                                                                                                                                                                                          |                      |      |          |                                                                                                                                                                                                   |

## 复制文件/文件夹命令

1. 作用：将指定的文件或文件夹拷贝到另一文件或路径中。
2. 语法

```shell
   cp [optional parameter] [source file/folder path] [target file/folder path]
```

3. 参数| 编号 | 参数名称 | 作用                                                                                                   | 说明 |
   | ---- | -------- | ------------------------------------------------------------------------------------------------------ | ---- |
   | 1    | -a       | 该选项通常在拷贝目录时使用。它保留链接、文件属性，并递归地拷贝目录，其作用等于dpR选项的组合。          | /    |
   | 2    | -d       | 拷贝时保留链接。                                                                                       | /    |
   | 3    | -f       | 删除已经存在的目标文件而不提示。                                                                       | /    |
   | 4    | -i       | 和f选项相反，在覆盖目标文件之前将给出提示要求用户确认。回答y时目标文件将被覆盖，是交互式拷贝。         | /    |
   | 5    | -p       | 此时cp除复制源文件的内容外，还将把其修改时间和访问权限也复制到新文件中。                               | /    |
   | 6    | -r       | 若给出的源文件是一目录文件，此时cp将递归复制该目录下所有的子目录和文件。此时目标文件必须为一个目录名。 | /    |
   | 7    | -l       | 不作拷贝，只是链接文件。                                                                               | /    |
4. 说明：为防止用户在不经意的情况下用cp命令破坏另一个文件，如用户指定的目标文件名已存在，用cp命令拷贝文件后，这个文件就会被新源文件覆盖，因此，建议用户在使用cp命令拷贝文件时，最好使用i选项。
5. 示例

   1. 拷贝文件。将/home/wally/test中 test.c 的文件复制到/local/arm中，命令为：

   ```shell
   cp -i /home/wally/test/test.c /local/arm
   ```

   2. 复制文件夹。将/home/wally/test目录拷贝到/local/arm目录中，命令为：

   ```shell
   cp -r /home/wally/test /local/arm
   ```

## 查看挂载情况

1. df -h
2. mount
3. reference: <>

## 查看路径

1. whereis python
2. which python

## 删除文件

1. 删除文件
   ```shell
   rm [filename]
   ```

## 创建文件。参考[https://blog.csdn.net/qq_39091609/article/details/106878845](https://blog.csdn.net/qq_39091609/article/details/106878845)。

```shell
   touch [filename]
```

   创建多个文件：

```shell
   touch [filename1], [filename2], [filename3]
```

## 创建用户 useradd

   参考:[https://blog.csdn.net/lele52141/article/details/6593840](https://blog.csdn.net/lele52141/article/details/6593840)。

1. 作用
   useradd命令用来建立用户帐号和创建用户的起始目录，使用权限是超级用户。
2. 格式
   useradd [－d home] [－s shell] [－c comment] [－m [－k template]] [－f inactive] [－e expire ] [－p passwd] [－r] name
3. 主要参数
   －c：加上备注文字，备注文字保存在passwd的备注栏中。　
   －d：指定用户登入时的启始目录。
   －D：变更预设值。
   －e：指定账号的有效期限，缺省表示永久有效。
   －f：指定在密码过期后多少天即关闭该账号。
   －g：指定用户所属的群组。
   －G：指定用户所属的附加群组。
   －m：自动建立用户的登入目录。
   －M：不要自动建立用户的登入目录。
   －n：取消建立以用户名称为名的群组。
   －r：建立系统账号。
   －s：指定用户登入后所使用的shell。
   －u：指定用户ID号。
4. 说明
   useradd可用来建立用户账号，它和adduser命令是相同的。账号建好之后，再用passwd设定账号的密码。使用useradd命令所建立的账号，实际上是保存在/etc/passwd文本文件中。
5. 应用实例
   建立一个新用户账户，并设置ID：
   ＃useradd caojh －u 544

   需要说明的是，设定ID值时尽量要大于500，以免冲突。因为Linux安装后会建立一些特殊用户，一般0到499之间的值留给bin、mail这样的系统账号。

   EXAMPLE:

   在终端里执行以下命令：

```shell
   useradd -d /home/"username" -g "gid" -u "uid" -m -s /bin/bash "username"
   passwd "username"
```

   “username"自己指定， ”gid"必须是现有的组id，“uid"必须目前未被使用
   /etc/group文件里有所有组信息。以下命令可以创建新组：

```shell
   groupadd -g "gid" "group name"
```

## 删除用户

```shell
   userdel [username]
```

   ubuntu删除用户同样是在终端下操作的，需要注意的是，如果要删除的用户当前已登陆，是删除不掉的，必须注销掉当前用户切换为另一个用户下，才能删除。举个例子，刚才我新建立了一个用户为yang的用户，例如我现在用用户yang登陆了桌面，此时如果我想删除 yang 这个用户，是删除不掉的。正确的操作方法是，我注销掉 yang，然后使用 root 登陆到桌面，再删除 yang 即可。
   删除ubuntu用户的命令比较容易记：sudo userdel username，例如我想删除yang，则输入：sudo userdel yang，删除成功后，系统无任何提示。

| 编号 | 参数名称 | 作用 | 说明 |
| ---- | -------- | ---- | ---- |
| 1    |          |      | /    |
| 2    |          |      | /    |
| 3    |          |      | /    |
| 4    |          |      | /    |
| 5    |          |      | /    |
| 6    |          |      | /    |
|      |          |      | /    |
|      |          |      | /    |
|      |          |      | /    |

| 编号 | 命令                                                              | 简要作用                                                                                     | 说明 |
| ---- | ----------------------------------------------------------------- | -------------------------------------------------------------------------------------------- | ---- |
| 1    | clear                                                             | 清屏                                                                                         | /    |
| 2    | clear --help                                                      | cmd --help:查看Linux命令的帮助                                                               | /    |
| 3    | history                                                           | 查看终端输入的所有历史命令                                                                   | /    |
| 4    | Ctrl+Alt+T                                                        | 打开新的终端                                                                                 | /    |
| 5    | Ctrl+shift+T                                                      | 打开新的终端Tab                                                                              | /    |
| 6    | Ctrl+shift+n                                                      | 打开新的同目录的终端                                                                         | /    |
|      | cd ~                                                              | 回到用户目录。                                                                               | /    |
|      | cd Desktop                                                        | 回到桌面路径。                                                                               | /    |
|      | cd ..                                                             | 返回上一级。                                                                                 | /    |
|      | pwd                                                               | 查看当前路径。                                                                               | /    |
|      | mkdir [FolderName]                                                | 在当前目录下创建文件夹。                                                                     | /    |
|      | mkdir -p test2/test3/test4                                        | 创建多级文件夹。-p参数确保目录名称存在，不存在的就建一个。                                   | /    |
|      | touch [FileEntireName]                                            | 创建文件（如果存在只会更新创建时间）                                                         | /    |
|      | touch [FileEntireName]                                            | 创建test1.txt文件                                                                            | /    |
|      | ls                                                                | ls查看当前目录下所有文件及文件夹                                                             | /    |
|      | ls -l                                                             | ls -l 查看当前目录下所有文件及文件夹并显示详细信息（字节为单位）                             | /    |
|      | ls -a                                                             | ls -a 查看当前目录下所有文件及文件夹(包括隐藏的)                                             | /    |
|      | ls -lh                                                            | ls -lh 查看当前目录下所有文件及文件夹并显示详细信息(单位kMGT)                                | /    |
|      | ll==ls -laf                                                       |                                                                                              | /    |
|      | ls [path]                                                         | 罗列[path]路径下的所有文件及文件夹                                                           | /    |
|      | tree                                                              | tree：显示当前文件树结构 sudo apt-get install tree                                           | /    |
|      | rm**.**                                                           | 移除文件                                                                                     | /    |
|      | rm -r ***                                                         | 移除文件夹                                                                                   | /    |
|      | rm -rf ***                                                        | 移除文件夹及其下的所有文件                                                                   | /    |
|      | mv**.**/*** ***                                             | 移动文件夹/文件至***                                                                         | /    |
|      | cp**.**/*** ***                                             | 复制----------------                                                                         | /    |
|      | cp /home/zard/test_sh/test2 /home/zard/test_sh/test/test -r       |                                                                                              | /    |
|      | cp /home/zard/test_sh/test/test1.txt /home/zard/test_sh/test/test |                                                                                              | /    |
|      | tar czvf all.tar.gz testcom testcom2 tmp                          | c代表压缩，z代表使用gzip，v代表显示压缩日志，f代表指定压缩包名称，后面是要压缩的文件夹及文件 | /    |
|      | chmod 777 /文件夹目录                                             | chmod 命令可以更改文件夹权限。并不能更改文件夹权限,可以使用                                  | /    |
|      | chmod 777 *                                                       | 进入文件夹目录下,再使用该命令可以修改文件夹下所有文件夹和文件的权限。                        | /    |
|      | ls -l 文件夹路径                                                  | 查看文件和文件夹权限。                                                                       | /    |
||||/|
||||/|

常见权限意义：
-rw------- (600) 只有所有者才有读和写的权限
-rw-r–r-- (644) 只有所有者才有读和写的权限，组群和其他人只有读的权限
-rwx------ (700) 只有所有者才有读，写，执行的权限
-rwxr-xr-x (755) 只有所有者才有读，写，执行的权限，组群和其他人只有读和执行的权限
-rwx–x--x (711) 只有所有者才有读，写，执行的权限，组群和其他人只有执行的权限
-rw-rw-rw- (666) 每个人都有读写的权限
-rwxrwxrwx (777) 每个人都有读写和执行的权限

## 压缩/解压缩文件

mkdir testcom
touch testcom testcom2
cd testcom && touch testcom testcom2
cd ..
tree

### c代表压缩，z代表使用gzip，v代表显示压缩日志，f代表指定压缩包名称，后面是要压缩的文件夹及文件

tar czvf all.tar.gz testcom testcom2 tmp
rm -f testcom testcom2 tmp -r

### x代表解压缩

tar xzvf all.tar.gz

### 解压缩到目录

mkdir all
tar xzvf all.tar.gz -C all
tree

## zip格式

### 安装压缩工具 sudo apt-get install zip uzip

zip -r all2 testcom testcom2 tmp
unzip all2
mkdir all3
unzip all2 -d all3
