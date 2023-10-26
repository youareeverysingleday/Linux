# 在离线环境下搭建硬件包含GPU的Ubuntu系统

## 1. 总体思路

1. 使用Ubuntu启动盘安装Ubuntu。
2. 使用apt-mirror工具制作软件包的镜像，然后让ubuntu安装相关更新。

## 2. 参考

1. 重要-ubuntu下的apt内网本地源的正确搭建。使用apt-mirror工具<https://www.cnblogs.com/mlwork/p/12262819.html>。这个工具下载了整个ubuntu需要更新的软件源。大约有150GB的大小，需要下载很长时间。
   1. 需要详细说明。特别是mirror.list的使用。
2. 没有使用。制作少量软件源的参考。
   1. <https://blog.csdn.net/qq_41037945/article/details/124440867>。
   2. <https://zhuanlan.zhihu.com/p/346562578>。
3. 安装过程中的磁盘分区
   1. 需要详细说明。特别是挂载的步骤、方式、区别。

> 命令中的gedit就是ubuntu中的TextEditor工具。

## 3. 离线安装python的依赖库

1. 使用命令保存当前安装环境的Python依赖包。
   ```shell
   pip freeze > requirement.txt
   ```
   保存在/home/[Account]/路径下。
2. 使用命令将所有依赖库离线下载。
   ```shell
   pip download -d [SaveDependentsPath] -r [path of Requirement.txt]
   ```
   path of Requirement.txt: requirement.txt的保存路径。
   SaveDependentsPath: 将下载的依赖库保存的目录。

   示例：
   ```shell
   pip download -d /home/[Account]/PythonDependents/ -r /home/[Account]/requirement.txt
   ```
3. 会出现的问题：下载依赖包报错。解决方法：1. 换一个安装源。2. 最常用的方法-删除对应需要下载的python库，因为一般无法下载的库都不是python编译环境所强相关的库。

## 4. 使用apt-mirror建立离线apt安装源

### 4.1 references

1. 重点参考<https://blog.csdn.net/yanjiee/article/details/85011779>。
2. 重点参考2<https://www.cnblogs.com/mlwork/p/12262819.html>。

### 4.2 Steps

#### 4.2.1 下载源

1. 安装apt-mirror。这台服务器需要是能够连接互联网。
    ```shell
    sudo apt-get install apt-mirror
    ```
2. 配置apt-mirror下载相关信息。需要配置的是在/etc/apt/目录下的mirror.list文件。初次使用apt-mirror的时候该文件是不在的，直接创建一个mirror.list文件。其中仅对于Ubuntu 20.04而言的内容如下：
   ```txt
   ############# config ##################
   #
   set base_path    /home/[Account]/apt-mirror
   #
   # set mirror_path  $base_path/mirror
   # set skel_path    $base_path/skel
   # set var_path     $base_path/var
   # set cleanscript $var_path/clean.sh

   # 架构配置，i386/amd64，默认的话会下载跟本机相同的架构的源
   # set defaultarch  <running host architecture>
   # set postmirror_script $var_path/postmirror.sh
   # set run_postmirror 0

   # 下载线程数
   set nthreads     20
   set _tilde 0
   #
   ############# end config ##############
   # 这里没有添加deb-src的源。
   # 下面使用的清华的源为：https://mirrors.tuna.tsinghua.edu.cn/ubuntu
   deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu focal main restricted universe multiverse
   deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu focal-security main restricted universe multiverse
   deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu focal-updates main restricted universe multiverse
   # deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu focal-proposed main restricted universe multiverse
   # deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu focal-backports main restricted universe multiverse

   # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu focal main restricted universe multiverse
   # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu focal-security main restricted universe multiverse
   # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu focal-updates main restricted universe multiverse
   # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu focal-proposed main restricted universe multiverse
   # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu focal-backports main restricted universe multiverse

   clean https://mirrors.tuna.tsinghua.edu.cn/ubuntu
   ```
3. 在使用apt-mirror最后会出现下列提示信息：
   ```shell
   Running the Post Mirror script …
   (/home/[Account]/apt-mirror/var/postmirror.sh)

   /bin/sh: 0: cannot open /home/[Account]/apt-mirror/var/postmirror.sh: No such file

   Post Mirror script has completed. See above output for any possible errors.
   ```
   解决方法为：创建这个文件解决这个报错。
   ```shell
   touch /home/[Account]/apt-mirror/var/postmirror.sh
   ```
4. 下载的mirror文件夹非常大。使用tar命令打包之后再进行拷贝。
   ```shell
   tar -cvf [TargetTarFileName] [SourceFolderPath]
   ```
   举例，在apt-mirror-focal相对目录下使用该命令：
   ```shell
   tar -cvf apt-mirror-focal.tar apt-mirror-focal
   ```
5. 重点：apt-mirror下载完成之后使用apt-update会出现"icon-64x64@2.tar.gz"不存在的报错，其解决方法如下。解决方案的参考为：<https://github.com/apt-mirror/apt-mirror/issues/113>。
    只下载icon-64x64和相关的一些文件使用的脚本。创建一个.sh文件，然后填写内容：
    ```shell
    for dist in focal focal-updates focal-security; do 
        for comp in main  multiverse universe; do
            for size in 48 64 128; do
                wget http://mirrors.tuna.tsinghua.edu.cn/ubuntu/dists/${dist}/${comp}/dep11/icons-${size}x${size}@2.tar.gz -O ${dist}/${comp}/dep11/icons-${size}x${size}@2.tar.gz; 
            done
        done
    done
    ```

    可以替代apt-mirror命令的脚本如下，而且该脚本直接解决了无法下载icon-64x64的问题。
    使用注意：
    1. 如果需要将下列脚本应用在ubuntu其他的版本中时需要修改对应的版本代号。
    2. 需要修改对应的国内下载源。
    3. 需要修改其中对应的本地下载路径。
            
    ```shell
        #!/bin/bash

        # combined solutions for apt mirror from: https://github.com/apt-mirror/apt-mirror/issues/49,https://github.com/apt-mirror/apt-mirror/issues/102
        # I had to create this file, in order to solve the issue of not downloading DEP-11 @2 files
        # so all what I'm doing is running apt mirror manually, then downloading the other icon files every time
        # Grep will grep the line starting with "set base_path"
        # then we trim all extra white spaces
        # then we cut the string by delimiter white space and take third value

        dataFolder=$(grep -F "set base_path" /etc/apt/mirror.list | tr -s " " | cut -d' ' -f3)
        echo "Base Folder path set in /etc/apt/mirror.list is: $dataFolder"
        apt-mirror
        echo 
        echo -n "Do you want to Check MD5 Sum and Download Failed (auto Y in 5 seconds)? [Y/n]"
        echo
        read -t 5 answer
        exit_status=$?
        if [ $exit_status -ne 0 ] || [ "$answer" != "${answer#[Yy]}" ];then
            FAILEDPACKAGES=""
            echo "Reading and Checking MD5 checksum using file: $dataFolder/var/MD5"
            #cd $dataFolder/mirror
            rm -f FAILED_MD5.txt
            echo "Failed File will be stored in: $(PWD)/FAILED_MD5.txt"
            while IFS='' read -r line || [[ -n "$line" ]]; do
                #echo "Checking: $line"
                sum=$(echo $line | cut -d' ' -f1)
                filename=$(echo $line | cut -d' ' -f2)
                echo "$sum $dataFolder/mirror/$filename" | md5sum -c -
                RESULT=$?
                if [ $RESULT -ne 0 ];then
                    echo "$dataFolder/mirror/$filename" >> FAILED_MD5.txt
                    wget -O $dataFolder/mirror/$filename $filename
                    echo "$sum $dataFolder/mirror/$filename" | md5sum -c -
                    SUBRESULT=$?
                    if [ $SUBRESULT -ne 0 ];then
                        echo "Sorry failed checksum again for file: $dataFolder/mirror/$filename"
                        $FAILEDPACKAGES+="$dataFolder/mirror/$filename      Failed again, sorry cannot help"
                    fi
                fi
            done < "$dataFolder/var/MD5"
            #md5sum -c $dataFolder/var/MD5 | grep --line-buffered -i "FAILED" | tee FAILED_MD5.txt
            echo $FAILEDPACKAGES
            break
        else
            echo "ok I will not check MD5"
        fi
        echo -n "Do you want Download DEP-11 @ 2 icons (auto Y in 5 seconds)? [Y/n]"
        echo
        read -t 5 answer
        exit_status=$?
        if [ "$answer" != "${answer#[Yy]}" ] || [ $exit_status -ne 0 ] ;then
            cd $dataFolder/mirror/us.archive.ubuntu.com/ubuntu/dists
            for dist in focal focal-updates focal-security; do 
            for comp in main  multiverse universe; do
                for size in 48 64 128; do
                wget http://us.archive.ubuntu.com/ubuntu/dists/${dist}/${comp}/dep11/icons-${size}x${size}@2.tar.gz -O ${dist}/${comp}/dep11/icons-${size}x${size}@2.tar.gz; 
                done
            done
            done
        else
            echo "ok no downloading for icons"
        fi
        echo -n "Do you want to run Clean Script (auto Y in 5 seconds)? [Y/n]"
        echo
        read -t 5 answer
        exit_status=$?
        if [ "$answer" != "${answer#[Yy]}" ] || [ $exit_status -ne 0 ] ;then
            $dataFolder/var/clean.sh
        fi
    ```

#### 4.2.2 离线安装源

1. 后面几个参数是对软件包的分类（Ubuntu下是 main  restricted  universe  multiverse 这四个）
    main:完全的自由软件。
    restricted:不完全的自由软件。
    universe:ubuntu官方不提供支持与补丁，全靠社区支持。
    muitiverse：非自由软件，完全不提供支持和补丁

2. ubuntu的长期维护版本（LTS）的版本代号对照表
   |版本号|Codename|
   |---|---|
   |22.04|Jammy|
   |20.04|focal|
   |18.04|bionic|
   |16.04|xenial|
   |14.04|trusty|
   |12.04|precise|

3. 查看系统架构 uname -m
4. 查看软件架构 dpkg  --print-architecture .该命令用于显示本机的architecture，我在不同的机器上得到的结果有：arm64或amd64
5. sudo dpkg --print-foreign-architectures  外部架构，还不太清楚具体用法。
6. apt apt-get apt-config apt-cache的区别。apt是apt-get apt-config apt-cache最常用命令选项的集合。
7. 配置apt-get的配置文件sources.list时需要注意以下几点：
   1. sources.list的名称后面有复数s结尾。
   2. sources.list中对应本地文件夹的配置为
      ```shell
      deb [arch=amd64 trusted=yes] file:///home/[Acoount]/apt-mirror-focal/mirror/mirrors.tuna.tsinghua.edu.cn/ubuntu focal main restricted universe mutliverse
      deb [arch=amd64 trusted=yes] file:///home/[Acoount]/apt-mirror-focal/mirror/mirrors.tuna.tsinghua.edu.cn/ubuntu focal-updates main restricted universe mutliverse
      deb [arch=amd64 trusted=yes] file:///home/[Acoount]/apt-mirror-focal/mirror/mirrors.tuna.tsinghua.edu.cn/ubuntu focal-security main restricted universe mutliverse
      deb [arch=amd64 trusted=yes] file:///home/[Acoount]/apt-mirror-focal/mirror/mirrors.tuna.tsinghua.edu.cn/ubuntu focal-backports main restricted universe mutliverse
      deb [arch=amd64 trusted=yes] file:///home/[Acoount]/apt-mirror-focal/mirror/mirrors.tuna.tsinghua.edu.cn/ubuntu focal-proposed main restricted universe mutliverse
      ```
      其中需要说明的内容为：
         1. arch=amd64表示为软件架构。注意这里和系统架构是通过两个不同的命令查看。
         2. 本地安装源一定要添加trusted=yes。有了这个才不会报不信任安装源的错误。
         3. file:///home/[Acoount]/apt-mirror-focal/mirror/mirrors.tuna.tsinghua.edu.cn/ubuntu 这是通过apt-mirror下载安装源路径下的一个子目录。注意，apt-mirror下载的4级完整路径是：
            ```text
            .
            └─mirror
               ├─mirrors.tuna.tsinghua.edu.cn
               │  └─ubuntu
               │     ├─dists
               │     └─pool
               ├─skel
               │  └─mirrors.tuna.tsinghua.edu.cn
               │     └─ubuntu
               │        └─dists
               └─var
                  ├─...
                  ...
            ```
            而在apt-mirror中配置的下载镜像文件内容为："set base_path    /home/[Account]/apt-mirror-focal"，这个路径是下载了完整的mirror目录。
            关键点在于apt在进行apt-get update的时候使用的路径并不能够直接搜索"/home/[Account]/apt-mirror-focal"路径，而搜索的是"/home/[Acoount]/apt-mirror-focal/mirror/mirrors.tuna.tsinghua.edu.cn/ubuntu"目录。之所以apt-get update一直报如下错误的原因就是因为设置的路径想当然的认为是"/home/[Account]/apt-mirror-focal"的路径。
            ```text
            ```
            直到发现apt-get尝试去找的目录和设置的sources.list的目录好像非常别扭。所以修改了设置之后就可以消除上面报错之中的大部分报错。


            

            
