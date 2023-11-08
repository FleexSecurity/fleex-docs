This command allows you to send files and folders to the vps you spawned. However, please ensure that you have previously generated VPS using [`fleex spawn`](spawn.md) before utilizing this command.

```
Send a file/folder to a fleet using SCP

Usage:
  fleex scp [flags]

Flags:
  -d, --destination string   Destination file / folder
  -h, --help                 help for scp
  -n, --name string          Fleet name (default "pwn")
      --port int             SSH port (default -1)
  -p, --provider string      Service provider
  -s, --source string        Source file / folder
  -U, --username string      Username

Global Flags:
      --config string     config file
  -l, --loglevel string   Set log level. Available: debug, info, warn, error, fatal (default "info")
      --proxy string      HTTP Proxy (Useful for debugging. Example: http://127.0.0.1:8080)
```


Examples:

Send file to /tmp
```
fleex scp -n FLEET-NAME -s ~/something.txt -d /tmp
```
Result:
```
INFO[0000] SCP completed, you can find your files in /home/root 
```

--- 

Send folder to /tmp
```
fleex scp -n FLEET-NAME -s ~/something/ -d /tmp
```
Result:
```
INFO[0000] SCP completed, you can find your files in /home/root 
```
