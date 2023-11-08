The fleex delete command empowers you to efficiently remove existing fleets or individual boxes from your provider. This command offers flexibility in managing your resources.
Usage:

```
Delete an existing fleet or even a single box

Usage:
fleex delete [flags]

Flags:
-h, --help              help for delete
-n, --name string       Fleet name. Boxes will be named [name]-[number] (default "pwn")
-p, --provider string   Service provider

Global Flags:
    --config string     config file
-l, --loglevel string   Set log level. Available: debug, info, warn, error, fatal (default "info")
    --proxy string      HTTP Proxy (Useful for debugging. Example: http://127.0.0.1:8080)

```


Examples:
```
fleex delete
```
Or
```
fleex delete -p PROVIDER-NAME -n FLEET-NAME
```

!!! note
    You can delete the vps individually by entering the full name