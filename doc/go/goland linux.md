#### root步骤

*   `tar -zxvf goland-2020.1.1.tar.gz -C /usr/local`
*   `sudo mv /usr/local/GoLand-2020.1.1/ /usr/local/GoLand`
*   `cd /usr/local/GoLand/bin/`
*   `sudo ln -s \$(pwd)/goland.sh /usr/bin/goland.sh`
*   `vim .bashrc`
*   `alias goland='nohup goland.sh & >/dev/null'`
*   `cd /home/yy/Downloads`
*   `mkdir ~/.config/JetBrains/GoLand2020.1/colors`
*   `cp goColor.icls \~/.config/JetBrains/GoLand2020.1/colors/`
*   `mkdir ~/.config/JetBrains/GoLand2020.1/keymaps`
*   `cp golandKeymap.xml ~/.config/JetBrains/GoLand2020.1/keymaps/`

#### 配置路径

*   `\~/.config/JetBrains/GoLand2020.1`

#### 插件

*   TranslationPlugin-2.9.2
*   cmdsupport

## 白嫖

<https://github.com/libin9iOak/ja-netfilter-all>
