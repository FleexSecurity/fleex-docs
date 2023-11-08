!!! warning
    Still in WIP, some things on this page may be subject to change

This command allows you to running distributed commands among all the vps you have generated.  However, please ensure that you have previously generated VPS using [`fleex spawn`](spawn.md) before utilizing this command.


Usage:

```
Send a command to a fleet, but also with files upload & chunks splitting

Usage:
  fleex scan [flags]

Flags:
      --chunks-folder string   Output folder containing output chunks. If empty it will use /tmp/<unix_timestamp>
  -c, --command string         Command to send. Supports {{INPUT}} and {{OUTPUT}}
  -d, --delete                 Delete boxes as soon as they finish their job
  -h, --help                   help for scan
  -i, --input string           Input file
      --module string          Specify path to a YAML module file
  -n, --name string            Fleet name (default "pwn")
  -o, --output string          Output file path. Made from concatenating all output chunks from all boxes
      --params strings         Set parameters in the format KEY:VALUE
  -P, --password string        SSH password
      --port int               SSH port (default -1)
  -p, --provider string        VPS provider
  -U, --username string        SSH username

Global Flags:
      --config string     config file
  -l, --loglevel string   Set log level. Available: debug, info, warn, error, fatal (default "info")
      --proxy string      HTTP Proxy (Useful for debugging. Example: http://127.0.0.1:8080)
```


Examples:

```
fleex scan --module module.yaml
```

```
fleex scan --module module.yaml
  --params INPUT:new_wordlist.txt
  --params OUTPUT:new_scan_results.txt
  --params URL:https://example2.com/FUZZ
```

```yaml

# module.yaml

name: ffuf-module-test
author: FleexSecurity
description: ffuf module test

vars:
  INPUT: wordlist.txt
  OUTPUT: scan-results.txt
  URL: https://example.com/FUZZ

commands:
  - /root/go/bin/ffuf -w {vars.INPUT} -u {vars.URL} -o {vars.OUTPUT} -of csv
```

More info about modules also here: [fleex-modules](https://github.com/FleexSecurity/fleex-modules)

This will send the `/root/go/bin/ffuf -w wordlist.txt -u https://example.com/FUZZ -o scan-results.txt -of csv` command to all the boxes of your `pwn` fleet.

!!! note 
    `INPUT` and `OUTPUT` are special labels that gets adjusted accordingly for each box.
    Fleex will split the input file, in this case `/tmp/test-input` and send a chunk to each box.

Once every box has finished, all output files will be downloaded to your local machine and merged automatically: the final output will be the `scan-results.txt` file, the name passed to the `-o` flag.