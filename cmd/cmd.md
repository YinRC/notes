#### python 程序打包成exe可执行文件

````bash
pyinstaller -F setup.py # 打包exe
pyinstaller -F -w setup.py # 不带控制台的打包
pyinstaller -F -i xxx.ico setup.py # 打包指定exe图标打包
````

#### win 端口占用

```powershell
netstat -ano | findstr <端口号>
taskkill /PID <进程ID> /F
# 将 `<进程ID>` 替换为要杀掉的进程的实际PID。选项 `/F` 表示强制终止进程。
```

- `-a`：显示所有的连接和监听端口，包括TCP和UDP。
- `-n`：以数字形式显示IP地址和端口号，而不进行反向DNS解析。
- `-o`：显示与每个连接关联的进程ID（PID）。

#### 无线网络切换

```powershell
netsh wlan show interfaces # 查看当前连接的wifi
netsh wlan connect name="my_profile" #   
```

