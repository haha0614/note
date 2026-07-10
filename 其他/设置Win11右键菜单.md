使用快捷键Win+X，然后点击“Windows终端（管理员）”以打开管理员权限的命令提示符。
在命令提示符中输入以下命令，并按回车键：

```cmd
reg add "HKCU\Software\Classes\CLSID\\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /f /ve
如果操作成功，命令提示符会显示“操作成功完成”消息。
```



接着，在命令提示符中输入以下命令，并按下回车键：

```cmd
taskkill /f /im explorer.exe
这个命令用于强制关闭资源管理器进程。
```



然后，输入以下命令并按下回车键：

```cmd
start explorer.exe
这个命令用于启动一个新的资源管理器进程。
```



至此，Windows 11的右击菜单恢复到了以前的习惯。
反之如果需要恢复Windows 11默认右击菜单，重复以上的步骤，并将步骤二中的命令改为以下命令

```cmd
reg delete "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /va /f
```


