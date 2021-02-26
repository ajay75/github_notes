###Hashi Corp Packer

Immutable infrastructure : Unachanging over time/ unable to change
Apart from this Packers provides following benefits:
- Testable infrastructure
- Reproducable 
- Infrastructure is unit of Deployment
- Confident of changes, as you can always rollback if needed.

How Packer differ from docker 
- Docker also provides immuatable images, but docker images are container
  based images from Linux/Windows 2016/2019 server
- Packer build images based provider specific it can be amazon,   virtualBox, you can build simultanious images for cloud provider as well as for your local environment.
