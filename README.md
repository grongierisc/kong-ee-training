# InterSystems API Manager Training

This repository contains the materials, examples, excercices to learn the basic concepts of IAM.

# TOC



# Introduction

## What is IAM ? 

IAM stand for InterSystems API Manager, it's based on Kong Entreprise Edition.
This mean you have acces on top of Kong OpenSource edition to :
* Manager Protal
* Developer Portal 
* Advance plugin
  * Oauth2
  * Caching
  * ...

![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/KongEEvsOSS.png)

## What is an API Managment ?

API management is the process of creating and publishing web application programming interfaces (APIs), enforcing their usage policies, controlling access, nurturing the subscriber community, collecting and analyzing usage statistics, and reporting on performance. API Management components provide mechanisms and tools to support developer and subscriber community.

![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/Api_Managment.png)

## IAM Portal

Kong and IAM are design as API first, this mean, every thing does in Kong/IAM can be done by rest calls or the manager portal.

During this training all example / exercice will present both this way :

<table>
<tr>
<th> IAM Portal </th>
<th> Rest API </th>
</tr>
<tr>
<td>

![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/default_kong.png)

</td>
<td>

![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/rest_call.png)

</td>
</tr>
</table>


# What do you need to install? 
* [Git](https://git-scm.com/downloads) 
* [Docker](https://www.docker.com/products/docker-desktop) (if you are using Windows, make sure you set your Docker installation to use "Linux containers").
* [Docker Compose](https://docs.docker.com/compose/install/)
* [Visual Studio Code](https://code.visualstudio.com/download) + [InterSystems ObjectScript VSCode Extension](https://marketplace.visualstudio.com/items?itemName=daimor.vscode-objectscript)
* InterSystems IRIS IAM enabled license file.
* IAM Docker image

# Setup

## IRIS image
You need to setup your access to InterSystems Container Registry to download IRIS limited access images.

Have a look at this [Introducing InterSystems Container Registry](https://community.intersystems.com/post/introducing-intersystems-container-registry) on [Developer Community](https://community.intersystems.com).

* Log-in into https://containers.intersystems.com/ using your WRC credentials and get a *token*.
* Set up docker login in your computer:
```bash
docker login -u="user" -p="token" containers.intersystems.com
```
* Get InterSystems IRIS image:
```bash
docker pull containers.intersystems.com/intersystems/iris:2020.2.0.211.0
```

## IAM Image
In [WRC Software Distribution](https://wrc.intersystems.com/wrc/coDistribution.csp):
* Components > Download *IAM-1.5.0.9-4.tar.gz* file, unzip & untar and then load the image:
```bash
docker load -i iam_image.tar
```