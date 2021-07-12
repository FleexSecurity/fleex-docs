This command allows you to connect via ssh to a vps you have spawned.

!!! warning
    You must first have generated VPS with `fleex spawn` before you can use the ssh command.

Usage:

| Flag | Name            | Description                                                                | Default |
| ---- | --------------- | -------------------------------------------------------------------------- | ------- |
| `-n` | `--name`        | Fleet name. Boxes will be named [name]-[number]                            | pwn     |
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
    ```
    fleex ssh -n pwn-1 --port 22 -U root
    ```
    Or
    ```
    fleex ssh -n pwn-1
    ```
