This command allows you to send files and folders to the vps you spawned.

!!! warning
    You must first have generated VPS with `fleex spawn` before you can use the scp command.

Usage:

| Flag | Name            | Description                                                                | Default |
| ---- | --------------- | -------------------------------------------------------------------------- | ------- |
| `-d` | `--destination` | Destination file / folder                                                  |         |
| `-n` | `--name`        | Fleet name. Boxes will be named [name]-[number]                            | pwn     |
| `-P` | `--password`    | SSH password                                                               |         |
|      | `--port`        | SSH port                                                                   | -1      |
| `-p` | `--provider`    | Service provider (Supported: linode, digitalocean)                         |         |
| `-U` | `--username`    | SSH username                                                               |         |


Examples:
!!! warning
    Image used in the examples:

    - Linode: `linode/ubuntu20.04`
    - Digitalocean: `ubuntu-20-04-x64`

    If you want to use custom images (for example `$HOME/fleex/build/common.yaml`) remember to update your `config.yaml`, with the **port number**, **username** and **password**

    config.yaml:
    ```
    ...
        port: 22
        username: root
        password: 1337superPass
    ```


!!! example
    === "Send file to /tmp"
        ```
        fleex scp -n FLEET-NAME -s ~/something.txt -d /tmp
        ```
        Result:
        ```
        INFO[0000] SCP completed, you can find your files in /home/root 
        ```

    === "Send folder to /tmp"
        ```
        fleex scp -n FLEET-NAME -s ~/something/ -d /tmp
        ```
        Result:
        ```
        INFO[0000] SCP completed, you can find your files in /home/root 
        ```
