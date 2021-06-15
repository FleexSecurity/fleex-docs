# Configuration

!!! Note
    Default location: $HOME/.fleex.yaml

```yaml
provider: digitalocean / linode
public-ssh-file: id_rsa.pub
private-ssh-file: id_rsa

# Linode
linode:
  token: YOUR TOKEN
  region: eu-central
  linode-type: g6-nanode-1
  image: YOUR CUSTOM IMAGE / linode/ubuntu20.04
  port: 22
  username: USERNAME
  password: PASSWORD

# Digitalocean
digitalocean:
  token: YOUR TOKEN
  region: fra1
  ssh-fingerprint: YOUR SSH FINGERPRINT
  size: s-1vcpu-1gb
  slug: ubuntu-20-04-x64
  image-id: ID FOR CUSTOM IMAGE
  port: 22
  username: USERNAME
  password: PASSWORD
  tags:
    - vps
    - fleex
```