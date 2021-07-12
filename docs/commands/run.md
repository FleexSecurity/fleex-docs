This command allows you to execute commands inside spawned machines.

!!! warning
    You must first have generated VPS with `fleex spawn` before you can use the run command.

Usage:

| Flag | Name              | Description                                                                        | Default |
| ---- | ----------------- | ---------------------------------------------------------------------------------- | ------- |
| `-c` | `--command`       | Command to send                                 |         |
| `-n` | `--name`          | Fleet name. Boxes will be named [name]-[number]                                    | pwn     |
| `-P` | `--password`      | SSH password                                                                       |         |
|      | `--port`          | SSH port                                                                           | -1      |
| `-p` | `--provider`      | Service provider (Supported: linode, digitalocean)                                 |         |
| `-U` | `--username`      | SSH username                                                                       |         |

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

!!! example "Show all files in ~"
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

!!! example "Whoami"
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
