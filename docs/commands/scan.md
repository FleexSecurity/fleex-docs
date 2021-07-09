This command allows you to running distributed commands among all the vps you have generated.

!!! warning
    You must first have generated VPS with `fleex spawn` before you can use the scan command.

Usage:

| Flag | Name              | Description                                                                        | Default |
| ---- | ----------------- | ---------------------------------------------------------------------------------- | ------- |
|      | `--chunks-folder` | Output folder containing output chunks. If empty it will use /tmp/<unix_timestamp> |         |
| `-c` | `--command`       | Command to send. Supports {{INPUT}} and {{OUTPUT}}                                 |         |
| `-d` | `--delete`        | Delete boxes as soon as they finish their job                                      |         |
| `-i` | `--input`         | Input file                                                                         |         |
| `-n` | `--name`          | Fleet name. Boxes will be named [name]-[number]                                    | pwn     |
| `-o` | `--output`        | Output file path. Made from concatenating all output chunks from all boxes         |         |
| `-P` | `--password`      | SSH password                                                                       |         |
|      | `--port`          | SSH port                                                                           | -1      |
| `-p` | `--provider`      | Service provider (Supported: linode, digitalocean)                                 |         |
| `-U` | `--username`      | SSH username                                                                       |         |


Examples:

```
fleex scan -n pwn -i /tmp/test-input -o scan-results.txt -c "sudo /usr/bin/massdns -r /home/op/lists/resolvers.txt -t A -o S {{INPUT}} -w {{OUTPUT}}"
```

This will send the `sudo /usr/bin/massdns -r /home/op/lists/resolvers.txt -t A -o S {{INPUT}} -w {{OUTPUT}}` command to all the boxes of your `pwn` fleet.

!!! note 
    `{{INPUT}}` and `{{OUTPUT}}` are special labels that gets adjusted accordingly for each box.
    Fleex will split the input file, in this case `/tmp/test-input` and send a chunk to each box.

Once every box has finished, all output files will be downloaded to your local machine and merged automatically: the final output will be the `scan-results.txt` file, the name passed to the `-o` flag.