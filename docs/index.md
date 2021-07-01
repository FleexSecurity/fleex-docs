## Welcome to Fleex Docs

Fleex allows you to spawn multiple VPS on various cloud providers and later use them to run distributed commands.

We've built it mainly for security researchers and bug bounty hunters, allowing them to run internet scanning tools such as nmap, masscan, massdns and get results very quickly.

However, thank to its design, you can use Fleex to run any command you need to distribute.

## Supported providers

We currently support the following providers:
!!! example ""
    - Linode
    - Digitalocean

Note that in the future we will add more of them, but for now you should be good with these two as they are the most common ones.

## Installation

Fleex is written in Go, so you can easily install it with the following command:

```
GO111MODULE=on go get -v github.com/sw33tLie/fleex
```
Don't forget to set the `GOPATH` environment variable if you want to run fleex from any folder. 

### Initial setup

Fleex needs some configuration files to be downloaded on your local machine.
To get them, run the following command just after you've installed it:

```
fleex config init
```

This will fetch the configuration files from our GitHub release and put them in your `~/fleex/` folder.

The only file you need to worry about is `~/fleex/config.yaml`: open it with your favourite text editor, it will look like this:

```
provider: digitalocean or linode
public-ssh-file: id_rsa.pub
private-ssh-file: id_rsa
linode:
  token: YOUR_LINODE_TOKEN
  region: eu-central
  size: g6-nanode-1
  image: private/12345678 : put your image id here (./fleex images to get it)
  port: 2266
  username: op
  password: USER_PASSWORD
digitalocean:
  token: YOUR_DIGITALOCEAN_TOKEN
  region: fra1
  size: s-1vcpu-1gb
  image: 12345678 # put your image id here
  port: 2266
  username: op
  password: USER_PASSWORD
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

### Building an image

Fleex takes care of creating a new image if you need one.
It will install tools that are usually needed by infosec researchers/bug bounty hunters.
If you want to edit the tools that will be installed, manually edit the `~/fleex/build/common.yaml` file before proceeding.

Now, to actually start the building process, simply run:
```
fleex build
```

This will take some time, around 20 minutes on average.
The good news is that you will only have to do this once for every cloud provider you wish to use.

When it's done, run `fleex images`, get the image id and manually put it into the configuration file, then save it.

### Spawning new boxes

Once you have configured everything, the real fun begins.

To spawn a new fleet (aka, many boxes with starting with fleetname-box_id) run:
```
fleex spawn -n FLEET_NAME -c FLEET_COUNT
```
Remember that for any fleex subcommand, you can use the `-h` flag to get help regarding its usage.

Fleex will automatically wait until all boxes are ready before exiting.

### Running distributed commands

When your fleet is ready, you can finally run distributed commands.

We will start with an example command:
```
fleex scan -n pwn -i /tmp/test-input -o scan-results.txt -c "sudo /usr/bin/massdns -r /home/op/lists/resolvers.txt -t A -o S {{INPUT}} -w {{OUTPUT}}"
```

This will send the `sudo /usr/bin/massdns -r /home/op/lists/resolvers.txt -t A -o S {{INPUT}} -w {{OUTPUT}}` command to all the boxes of your `pwn` fleet.

Note that those `{{INPUT}}` and `{{OUTPUT}}` are special labels that gets adjusted accordingly for each box.
Fleex will split the input file, in this case `/tmp/test-input` and send a chunk to each box.

Once every box has finished, all output files will be downloaded to your local machine and merged automatically: the final output will be the `scan-results.txt` file, the name passed to the `-o` flag.