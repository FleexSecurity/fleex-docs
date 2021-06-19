# Configuration

!!! Info
    Default location: $HOME/fleex/config.yaml

### Default config.yaml

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
  size: s-1vcpu-1gb
  slug: ubuntu-20-04-x64
  image: ID FOR CUSTOM IMAGE
  port: 22
  username: USERNAME
  password: PASSWORD
  tags:
    - vps
    - fleex
```

## Linode

### Regions
!!! example ""
    - **eu-central**: Frankfurt, DE
    - **eu-west**: London, UK
    - **us-southeast**: Atlanta, GA
    - **us-central**: Dalas, TX     
    - **us-west**: Fremont, CA   
    - **us-east**: Newark, NJ    
    - **ca-central**: Toronto, ON   
    - **ap-south**: Singapore, SG 
    - **ap-northeast**: Tokyo 2, JP   
    - **ap-west**: Mumbai, IN    
    - **ap-southeast**: Sydney, AU
    ??? Info -
        More info on [Linode API](https://api.linode.com/v4/regions)

### Types
!!! example ""
    - **g6-nanode-1**: Nanode 1GB - 5$/mo, 0.0075$/hr 
    - **g6-standard-1**: Linode 2GB - 10$/mo, 0.015$/hr 
    - **g6-standard-2**: Linode 4GB - 20$/mo, 0.03$/hr 
    - **g6-standard-4**: Linode 8GB - 40$/mo, 0.06$/hr 
    - **g6-standard-6**: Linode 16GB - 80$/mo, 0.12$/hr 
    - **g6-standard-8**: Linode 32GB - 160$/mo, 0.24$/hr 
    - **g6-standard-16**: Linode 64GB - 320$/mo, 0.48$/hr 
    - **g6-standard-20**: Linode 96GB - 480$/mo, 0.72$/hr
    - ...
    - ...
    ??? Info -
        More info on [Linode API](https://api.linode.com/v4/linode/types)

## Digitalocean

### Regions
!!! example ""
    - **NYC1, NYC2, NYC3**: New York City, United States
    - **AMS2, AMS3**: Amsterdam, the Netherlands
    - **SFO1, SFO2, SFO3**: San Francisco, United States
    - **SGP1**: Singapore
    - **LON1**: London, United Kingdom
    - **FRA1**: Frankfurt, Germany
    - **TOR1**: Toronto, Canada  
    - **BLR1**: Bangalore, India
    ??? Info -
        More info on [Digitalocean regional availability matrix ](https://docs.digitalocean.com/products/platform/availability-matrix/)

### Types
!!! example ""
    - **s-1vcpu-1gb**: Standard - 5$/mo
    - **s-1vcpu-2gb**: Standard - 10$/mo 
    - **s-1vcpu-3gb**: Standard - 15$/mo
    - **s-2vcpu-4gb**: Standard - 20$/mo
    - **s-4vcpu-8gb**: Standard - 40$/mo 
    - **s-6vcpu-16gb**: Standard - 80$/mo 
    - **s-8vcpu-32gb**: Standard - 160$/mo 
    - ...
    - ...
    ??? Info -
        More info on [Digitalocean droplet plan](https://developers.digitalocean.com/documentation/changelog/api-v2/new-size-slugs-for-droplet-plan-changes/)