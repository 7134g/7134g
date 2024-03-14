## 如何修改Windows 无后缀文件，设置默认打开方式

cmd执行

`assoc .=No Extension`

然后执行下面，里面的 C:\Windows\System32\notepad.exe  更改为你实际要用的软件路径。

`ftype "No Extension"="C:\Windows\System32\notepad.exe""%1"`

