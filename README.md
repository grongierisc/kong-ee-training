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

## Flow of this training

The aim of this training is to use IAM as a proxy of an IRIS rest API.

Definition of this rest API can be found here : 

```url
http://localhost:52773/swagger-ui/index.html#/
```
or here
```url
https://github.com/grongierisc/iam-training/blob/training/misc/spec.yml
```

Start this training with the main branch.
At the end of the training you should have the same result as the training branch.
# Installation
## What do you need to install? 
* [Git](https://git-scm.com/downloads) 
* [Docker](https://www.docker.com/products/docker-desktop) (if you are using Windows, make sure you set your Docker installation to use "Linux containers").
* [Docker Compose](https://docs.docker.com/compose/install/)
* [Visual Studio Code](https://code.visualstudio.com/download) + [InterSystems ObjectScript VSCode Extension](https://marketplace.visualstudio.com/items?itemName=daimor.vscode-objectscript)
* InterSystems IRIS IAM enabled license file.
* IAM Docker image

## How IAM work's with IRIS

At Kong/IAM start, the container check for the Kong/IAM license with a curl call.
The endpoint of this call is a rest API on the IRIS container.
FYI : Kong license is embedded in IRIS one.

![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/IAM_IRIS.png)
## Setup

Git clone this repository.

```sh
git clone https://github.com/grongierisc/iam-training
```

Run the initial rest API :

```sh
docker-compose up
```

Test it :

```url
http://localhost:52773/swagger-ui/index.html#/
```

Login/Password :
SuperUser/SYS

## Install IAM

### Iris Iamge

First you need to swith for the community edition to an licensed one.

To do so, you need to setup your access to InterSystems Container Registry to download IRIS limited access images.

Have a look at this [Introducing InterSystems Container Registry](https://community.intersystems.com/post/introducing-intersystems-container-registry) on [Developer Community](https://community.intersystems.com).

* Log-in into https://containers.intersystems.com/ using your WRC credentials and get a *token*.
* Set up docker login in your computer:
```bash
docker login -u="user" -p="token" containers.intersystems.com
```
* Get InterSystems IRIS image:
```bash
docker pull containers.intersystems.com/intersystems/irishealth:2020.4.0.524.0
```

### IAM Image
In [WRC Software Distribution](https://wrc.intersystems.com/wrc/coDistribution.csp):
* Components > Download *IAM-1.5.0.9-4.tar.gz* file, unzip & untar and then load the image:
```bash
docker load -i iam_image.tar
```

### Update the docker file

Change IRIS community edition to a licensed one.
* containers.intersystems.com/intersystems/irishealth:2020.4.0.524.0
* Add iris.key in key folder

Edit the dockerfile to add on top of it this part
```dockerfile
ARG IMAGE=containers.intersystems.com/intersystems/irishealth:2020.4.0.524.0
FROM $IMAGE as iris-iam
COPY key/iris.key /usr/irissys/mgr/iris.key
COPY iris-iam.script /tmp/iris-iam.script
RUN iris start IRIS \
&& iris session IRIS < /tmp/iris-iam.script \
&& iris stop IRIS quietly
FROM iris-iam
```

Create a new file iris-iam.script to build a new IRIS Image to enable IAM endpoint and user.

```objectscript
zn "%SYS"
wri"Create web application ...",!
set webName = "/api/iam"
set webProperties("Enabled") = 1
set status = ##class(Security.Applications).Modify(webName, .webProperties)
write:'status $system.Status.DisplayError(status)
write "Web application "_webName_" was updated!",!

set userProperties("Enabled") = 1
set userName = "IAM"
Do ##class(Security.Users).Modify(userName,.userProperties)
write "User "_userName_" was updated!",!
halt
```

### Update the docker-compose

Update the docker-compose file to :

* db
  * postgres database for IAM
* iam-migration
  * bootstrap the database
* iam
  * actual IAM instance
* a volume for data persistent 

Add this part to the end of the docker-compose file.

```docker-compose
  iam-migrations:
    image: intersystems/iam:1.5.0.9-4
    command: kong migrations bootstrap up
    depends_on:
      - db
    environment:
      KONG_DATABASE: postgres
      KONG_PG_DATABASE: ${KONG_PG_DATABASE:-iam}
      KONG_PG_HOST: db
      KONG_PG_PASSWORD: ${KONG_PG_PASSWORD:-iam}
      KONG_PG_USER: ${KONG_PG_USER:-iam}
      KONG_CASSANDRA_CONTACT_POINTS: db
      KONG_PLUGINS: bundled,jwt-crafter
      ISC_IRIS_URL: IAM:${IRIS_PASSWORD}@iris:52773/api/iam/license
    restart: on-failure
    links:
      - db:db
  iam:
    image: intersystems/iam:1.5.0.9-4
    depends_on:
      - db
    environment:
      KONG_ADMIN_ACCESS_LOG: /dev/stdout
      KONG_ADMIN_ERROR_LOG: /dev/stderr
      KONG_ADMIN_LISTEN: '0.0.0.0:8001'
      KONG_ANONYMOUS_REPORTS: 'off'
      KONG_CASSANDRA_CONTACT_POINTS: db
      KONG_DATABASE: postgres
      KONG_PG_DATABASE: ${KONG_PG_DATABASE:-iam}
      KONG_PG_HOST: db
      KONG_PG_PASSWORD: ${KONG_PG_PASSWORD:-iam}
      KONG_PG_USER: ${KONG_PG_USER:-iam}
      KONG_PROXY_ACCESS_LOG: /dev/stdout
      KONG_PROXY_ERROR_LOG: /dev/stderr
      KONG_PORTAL: 'on'
      KONG_PORTAL_GUI_PROTOCOL: http
      KONG_PORTAL_GUI_HOST: '127.0.0.1:8003'
      KONG_ADMIN_GUI_URL: http://localhost:8002
      KONG_PLUGINS: bundled
      ISC_IRIS_URL: IAM:${IRIS_PASSWORD}@iris:52773/api/iam/license
    volumes: 
      - ./iam:/iam
    links:
      - db:db
    ports:
      - target: 8000
        published: 8000
        protocol: tcp
      - target: 8001
        published: 8001
        protocol: tcp
      - target: 8002
        published: 8002
        protocol: tcp
      - target: 8003
        published: 8003
        protocol: tcp
      - target: 8004
        published: 8004
        protocol: tcp
      - target: 8443
        published: 8443
        protocol: tcp
      - target: 8444
        published: 8444
        protocol: tcp
      - target: 8445
        published: 8445
        protocol: tcp
    restart: on-failure
  db:
    image: postgres:9.6
    environment:
      POSTGRES_DB: ${KONG_PG_DATABASE:-iam}
      POSTGRES_PASSWORD: ${KONG_PG_PASSWORD:-iam}
      POSTGRES_USER: ${KONG_PG_USER:-iam}
    volumes:
      - 'pgdata:/var/lib/postgresql/data'
    healthcheck:
      test: ["CMD", "pg_isready", "-U", "${KONG_PG_USER:-iam}"]
      interval: 30s
      timeout: 30s
      retries: 3
    restart: on-failure
    stdin_open: true
    tty: true
volumes:
  pgdata:
```

