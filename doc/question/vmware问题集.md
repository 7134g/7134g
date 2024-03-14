## 突然没有网络

    因为环境发生变化，比如插线变wifi
    https://www.cnblogs.com/rqb0/p/15720402.html

## tools
- 将压缩包移动到本机中
- 解压到任意地方

## manjaro
- 卸载open-vm-tools
    - sudo pacman -R open-vm-tools

- 下载vmwaretools补丁
    - git clone https://github.com/rasa/vmware-tools-patches.git

- 进入vmware-tools-patches目录
    - cd vmware-tools-patches

- 运行补丁
    - sudo ./patched-open-vm-tools.sh
- 重启
    - reboot


## 网络问题
https://blog.csdn.net/u013554213/article/details/79408084/