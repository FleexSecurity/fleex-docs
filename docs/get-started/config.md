# Configuration

Fleex needs some configuration files to be downloaded on your local machine.
To get them, see the [init command here](../commands/init.md)

This will fetch the configuration files from our GitHub release and put them in your `$HOME/.config/fleex/` folder.

The only file you need to worry about is `$HOME/.config/fleex/config.json`: open it with your favourite text editor, it will look like this:

### Default config.json

```json
{
    "settings": {
        "provider": "linode"
    },
    "ssh_keys": {
        "public_file": "id_rsa.pub",
        "private_file": "id_rsa"
    },
    "providers": {
        "linode": {
            "token": "YOUR_LINODE_TOKEN",
            "region": "eu-central",
            "size": "g6-nanode-1",
            "image": "private/12345678",
            "port": 2266,
            "username": "op",
            "password": "USER_PASSWORD"
        },
        "vultr": {
            "token": "YOUR_VULTR_TOKEN",
            "region": "atl",
            "size": "vc2-1c-1gb",
            "image": "1a2b3c4d-1a2b-1a2b-1a2b-1a2b3c4d5e",
            "port": 2266,
            "username": "op",
            "password": "USER_PASSWORD"
        }
    },
    "custom_vms": [
        {
            "provider": "utm",
            "instance_id": "i-customid2",
            "public_ip": "1.2.3.4",
            "ssh_port": 22,
            "username": "user",
            "password": "USER_PASSWORD",
            "key_path": "/path/to/your/private-key.pem",
            "tags": [
                "test",
                "production"
            ]
        },
        {
            "provider": "virtualbox",
            "instance_id": "i-customid3",
            "public_ip": "1.2.3.4",
            "ssh_port": 22,
            "username": "user",
            "password": "USER_PASSWORD",
            "key_path": "/path/to/your/private-key.pem",
            "tags": [
                "staging"
            ]
        }
    ]
}

```

You only need to manually edit a few words here.

First thing first, choose the provider you want to use by default and manually edit the `provider:` line accordingly.
Then get your cloud provider's token from their website and put it by either replacing `YOUR_LINODE_TOKEN`.
After that, save the edits to the file.

Now you need to figure out if you already have an image on your cloud provider.
An image is basically a backup of an old VPS, which might have custom tools installed.
To avoid installing the same tools over and over, we recommend spawning new Fleex boxes using a custom image, rather than a default Ubuntu image.

If you already have an image, just run `fleex images` to get a list of images, then get the image ID for the image you want to use and put it in the configuration file.

If you don't, you need to build a new image.