[TOC]

# Commond Experience

## Experience

1. 使用Ubuntu server版本下载地址<https://cn.ubuntu.com/server>。
2. ubuntu server安装图形界面<https://zhuanlan.zhihu.com/p/373773218>。如果你是出于学习和调研等实验性的目的，那么你可以进行这些操作。请不要在生产环境的服务器上添加 GUI。后续删除 GUI 时可能会导致依赖问题，有些情况会破坏系统。
   1. 首先升级apt：sudo apt update
   2. 安装图形界面：sudo apt install ubuntu-desktop
      1.  安装的是GNOME桌面，因为它是 Ubuntu 默认的桌面。
   3. 安装“显示管理器”或“登录管理器”的组件。这个工具的功能是在管理用户对话和鉴权时启动 显示服务器 并加载桌面。：sudo apt install lightdm
   4. 启动显示管理器并加载 GUI：sudo service lightdm start
      1. 检查当前的显示管理器：cat /etc/X11/default-display-manager
   5. 关闭GUI：sudo service lightdm stop
3. 删除GUI（请注意在某些情况下删除 GUI 可能会带来依赖问题，因此请备份好重要数据或创建一个系统快照）：
   1. sudo apt remove ubuntu-desktop
   2. sudo apt remove lightdm
   3. sudo apt autoremove
   4. sudo service lightdm stop
4. 安装输入法
   1. reference:<https://zhuanlan.zhihu.com/p/529892064>。
5. ubuntu desktop 20.04需要依赖Python，不要删除python3。直接会导致gnome也被删除。建议安装其他版本的Python，然后将修改指向。
   1. 参考:<https://zhuanlan.zhihu.com/p/403819436>。
   2. 切换默认版本命令：sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.9