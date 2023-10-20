# Commond Commands

|编号|类别|命令/语法|功能|举例|参数|参数含义|说明|
|---|---|---|---|---|---|---|---|
|1|Linux目录的创建与删除命令|cd [directory]|改变工作目录|cd ..|||该命令将当前目录改变至directory所指定的目录。若没有指定directory， 则回到用户的主目录。为了改变到指定目录，用户必须拥有对指定目录的执行和读权限。该命令可以使用通配符（通配符含义请参见第十章）。|
|||pwd|在Linux层次目录结构中，用户可以在被授权的任意目录下利用mkdir命令创建新目录，也可以利用cd命令从一个目录转换到另一个目录。然而，没有提示符来告知用 户目前处于哪一个目录中。要想知道当前所处的目录，可以使用pwd命令，该命令显示整个路径名。|pwd|||此命令显示出当前工作目录的绝对路径。|
|2||find|查找文件|find path expression|||find . -name 'srm*' #表示当前目录下查找文件名开头是字符串‘srm’的文件|
|3||||||||
|||||||||

## 复制文件/文件夹命令
   1. 作用：将指定的文件或文件夹拷贝到另一文件或路径中。
   2. 语法
   ```shell
   cp [optional parameter] [source file/folder path] [target file/folder path]
   ```
   3. 参数
    |编号|参数名称|作用|说明|
    |---|---|---|---|
    |1|-a|该选项通常在拷贝目录时使用。它保留链接、文件属性，并递归地拷贝目录，其作用等于dpR选项的组合。|/|
    |2|-d|拷贝时保留链接。|/|
    |3|-f|删除已经存在的目标文件而不提示。|/|
    |4|-i|和f选项相反，在覆盖目标文件之前将给出提示要求用户确认。回答y时目标文件将被覆盖，是交互式拷贝。|/|
    |5|-p|此时cp除复制源文件的内容外，还将把其修改时间和访问权限也复制到新文件中。|/|
    |6|-r|若给出的源文件是一目录文件，此时cp将递归复制该目录下所有的子目录和文件。此时目标文件必须为一个目录名。|/|
    |7|-l|不作拷贝，只是链接文件。|/|
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

## 创建文件。参考<https://blog.csdn.net/qq_39091609/article/details/106878845>。
   ```shell
   touch [filename]
   ```
   创建多个文件：
   ```shell
   touch [filename1], [filename2], [filename3]
   ```

## 创建用户 useradd
   参考:<https://blog.csdn.net/lele52141/article/details/6593840>。
   
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



|编号|参数名称|作用|说明|
|---|---|---|---|
|1|||/|
|2|||/|
|3|||/|
|4|||/|
|5|||/|
|6|||/|
||||/|
||||/|
||||/|