### 覆写磁盘
`cipher /w:D:\`

### 修改时钟
第一步
- 运行regedit
- HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Advanced

2. 第二步
- 新建DWORD（32位）值，命名为ShowSecondsInSystemClock

3. 第三步
- 修改值为1


查看版本
`Get-CimInstance -ClassName Win32_OperatingSystem | select Caption, Version`