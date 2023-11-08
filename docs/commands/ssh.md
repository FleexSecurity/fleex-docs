This command allows you to connect via ssh to a vps you have spawned. However, please ensure that you have previously generated VPS using [`fleex spawn`](spawn.md) before utilizing this command.

Usage:

```
Start SSH terminal for a box

Usage:
  fleex ssh [flags]

Flags:
  -h, --help              help for ssh
  -n, --name string       Box name (default "pwn")
      --port int          SSH port (default -1)
  -p, --provider string   Service provider
  -U, --username string   SSH username

Global Flags:
      --config string     config file
  -l, --loglevel string   Set log level. Available: debug, info, warn, error, fatal (default "info")
      --proxy string      HTTP Proxy (Useful for debugging. Example: http://127.0.0.1:8080)
```

Examples:

```
fleex ssh -n pwn-1 --port 22 -U root
```
Or
```
fleex ssh -n pwn-1
```

Result 

```
Welcome to Ubuntu 18.04.6 LTS (GNU/Linux 4.15.0-213-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System load:  0.33              Processes:           98
  Usage of /:   6.5% of 24.04GB   Users logged in:     0
  Memory usage: 12%               IP address for eth0: 192.46.236.135
  Swap usage:   0%


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

root@localhost:~# ls -la
ls -la
total 28
drwx------  5 root root 4096 Nov  8 23:38 .
drwxr-xr-x 23 root root 4096 Sep  6 05:51 ..
-rw-r--r--  1 root root 3106 Apr  9  2018 .bashrc
drwx------  2 root root 4096 Nov  8 23:38 .cache
drwx------  3 root root 4096 Nov  8 23:38 .gnupg
-rw-r--r--  1 root root  148 Aug 17  2015 .profile
drwx------  2 root root 4096 Nov  8 23:36 .ssh
root@localhost:~# whoami
whoami
root
root@localhost:~# 
```