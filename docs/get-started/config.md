# Configuration

Fleex needs some configuration files to be downloaded on your local machine.
To get them, run the following command just after you've installed it:

```
fleex config init
```

This will fetch the configuration files from our GitHub release and put them in your `~/fleex/` folder.

The only file you need to worry about is `~/fleex/config.yaml`: open it with your favourite text editor, it will look like this:

### Default config.yaml

```yaml
# Supported providers
provider: digitalocean or linode
# SSH keys in $HOME/.ssh/
public-ssh-file: id_rsa.pub
private-ssh-file: id_rsa

# Settings for linode provider
linode:
  # Token: My profile > API Tokens > Create Personal Access Token
  token: YOUR_LINODE_TOKEN
  # Region & Size, see fleex-docs for more info
  region: eu-central
  size: g6-nanode-1
  # Put your image id here
  # Run "fleex images" to get it
  image: private/12345678
  # Settings for SSH command
  port: 2266
  username: op
  # You can set custom passwords for creating vps 
  password: USER_PASSWORD

# Settings for Digitalocean provider
digitalocean:
  # Token: API > Tokens/Keys > Generate New Token
  token: YOUR_DIGITALOCEAN_TOKEN
  # Region & Size, see fleex-docs for more info
  region: fra1
  size: s-1vcpu-1gb
  # Put your image id here
  # Run "fleex images" to get it
  # You can also insert a Slug here, like "ubuntu-20-04-x64"
  image: 12345678
  # Settings for SSH command
  port: 2266
  username: op
  # You can set custom passwords for creating vps 
  password: USER_PASSWORD
  # Optional
  tags:
    - vps
    - fleex

```

You only need to manually edit a few words here.

First thing first, choose the provider you want to use by default and manually edit the `provider:` line accordingly.
Then get your cloud provider's token from their website and put it by either replacing `YOUR_LINODE_TOKEN` or `YOUR_DIGITALOCEAN_TOKEN`.
After that, save the edits to the file.

Now you need to figure out if you already have an image on your cloud provider.
An image is basically a backup of an old VPS, which might have custom tools installed.
To avoid installing the same tools over and over, we recommend spawning new Fleex boxes using a custom image, rather than a default Ubuntu image.

If you already have an image, just run `fleex images` to get a list of images, then get the image ID for the image you want to use and put it in the configuration file.

If you don't, you need to build a new image.