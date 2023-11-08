The fleex run command empowers you to send commands to a fleet of spawned machines, enabling seamless remote execution of tasks. However, please ensure that you have previously generated VPS using [`fleex spawn`](spawn.md) before utilizing this command.

Usage:

```
Send a command to a fleet

Usage:
  fleex run [flags]

Flags:
  -c, --command string    Command to send
  -h, --help              help for run
  -n, --name string       Fleet name (default "pwn")
  -p, --port int          SSH port (default -1)
  -P, --provider string   Service provider
  -U, --username string   SSH username

Global Flags:
      --config string     config file
  -l, --loglevel string   Set log level. Available: debug, info, warn, error, fatal (default "info")
      --proxy string      HTTP Proxy (Useful for debugging. Example: http://127.0.0.1:8080)
```

!!! warning
    for this example 2 vps were used

Examples:

Remote ls -la
```
fleex run -c "ls -la"
```
Or
```
fleex run -c "ls -la" --port 22 -U root
```
Result:
```
total 28
drwx------  5 root root 4096 Jul 12 19:13 .
drwxr-xr-x 19 root root 4096 Jul 12 19:11 ..
-rw-r--r--  1 root root 3106 Dec  5  2019 .bashrc
drwx------  2 root root 4096 Jul 12 19:13 .cache
-rw-r--r--  1 root root    0 Jul 12 19:11 .cloud-locale-test.skip
-rw-r--r--  1 root root  161 Dec  5  2019 .profile
drwx------  2 root root 4096 Jul 12 19:11 .ssh
drwxr-xr-x  3 root root 4096 Jul 12 19:11 snap
total 28
drwx------  5 root root 4096 Jul 12 19:13 .
drwxr-xr-x 19 root root 4096 Jul 12 19:12 ..
-rw-r--r--  1 root root 3106 Dec  5  2019 .bashrc
drwx------  2 root root 4096 Jul 12 19:13 .cache
-rw-r--r--  1 root root    0 Jul 12 19:12 .cloud-locale-test.skip
-rw-r--r--  1 root root  161 Dec  5  2019 .profile
drwx------  2 root root 4096 Jul 12 19:12 .ssh
drwxr-xr-x  3 root root 4096 Jul 12 19:12 snap
```

---

Remote whoami
```
fleex run -c "whoami"
```
Or
```
fleex run -c "whoami" --port 22 -U root
```
Result:

```
root
root
```
