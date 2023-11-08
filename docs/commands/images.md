This command allows you to see all the images you have generated from a provider 

Usage:

```
List available images

Usage:
  fleex images ls [flags]

Flags:
  -h, --help              help for ls
  -p, --provider string   Service provider

Global Flags:
      --config string     config file
  -l, --loglevel string   Set log level. Available: debug, info, warn, error, fatal (default "info")
      --proxy string      HTTP Proxy (Useful for debugging. Example: http://127.0.0.1:8080)
```

Examples:
```
fleex images ls
```
Or
```
fleex images ls -p PROVIDER-NAME
```

Result:
```
<ID>     <NAME>                 <STATUS>  <SIZE (GB)>
12345678 Fleex-build-1624990254 available 10.52
23456789 Fleex-build-1624994814 available 5.42
34567890 Fleex-build-1625839588 available 5.86

```

!!! warning
    The displayed data is for example only