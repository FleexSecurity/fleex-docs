# fleex-docs

<a href="https://fleexsecurity.github.io/fleex-docs/"><img src="docs/img/Fleex-docs.png" alt="Fleex-docs"></a>

## Welcome to Fleex

Fleex allows you to spawn multiple VPS on various cloud providers and later use them to run distributed commands.

We've built it mainly for security researchers and bug bounty hunters, allowing them to run internet scanning tools such as nmap, masscan, massdns and get results very quickly.

However, thank to its design, you can use Fleex to run any command you need to distribute.

## Supported providers

We currently support the following providers:
>    - Linode
>    - Digitalocean


Note that in the future we will add more of them, but for now you should be good with these two as they are the most common ones.


## Available commands
```
./fleex -h

Available Commands:
  build       Build image
  config      fleex config setup
  delete      Delete a fleet or a single box
  help        Help about any command
  images      List available images
  ls          List running boxes
  run         Run a command
  scan        Distributed scanning
  scp         SCP client
  spawn       Spawn a fleet
  ssh         Start SSH
```

