# 1. InterSystems API Manager Training

This repository contains the materials, examples, excercices to learn the basic concepts of IAM.

- [1. InterSystems API Manager Training](#1-intersystems-api-manager-training)
- [2. Introduction](#2-introduction)
  - [2.1. What is IAM ?](#21-what-is-iam-)
  - [2.2. What is an API Managment ?](#22-what-is-an-api-managment-)
  - [2.3. IAM Portal](#23-iam-portal)
  - [2.4. Flow of this training](#24-flow-of-this-training)
- [3. Installation](#3-installation)
  - [3.1. What do you need to install?](#31-what-do-you-need-to-install)
  - [3.2. How IAM work's with IRIS](#32-how-iam-works-with-iris)
  - [3.3. Setup](#33-setup)
  - [3.4. Install IAM](#34-install-iam)
    - [3.4.1. Iris Image](#341-iris-image)
    - [3.4.2. IAM Image](#342-iam-image)
    - [3.4.3. Update the docker file](#343-update-the-docker-file)
    - [3.4.4. Update the docker-compose](#344-update-the-docker-compose)
    - [3.4.5. Option : add IRIS_PASSWARD as .env](#345-option--add-iris_passward-as-env)
    - [3.4.6. Test it !](#346-test-it-)
- [4. First Service/Route](#4-first-serviceroute)
  - [4.1. Create a service](#41-create-a-service)
  - [4.2. Create a route](#42-create-a-route)
  - [4.3. Test it !](#43-test-it-)
- [5. Second, Go futher with plugin](#5-second-go-futher-with-plugin)
  - [5.1. Add a plugin to the service](#51-add-a-plugin-to-the-service)
  - [5.2. Test it !](#52-test-it-)
- [6. Third, add our own authentication](#6-third-add-our-own-authentication)
  - [6.1. Add consumers](#61-add-consumers)
  - [6.2. Add Basic auth plugin](#62-add-basic-auth-plugin)
  - [6.3. Add ACL Plugin](#63-add-acl-plugin)
  - [6.4. Configure USER with ACL and credentials](#64-configure-user-with-acl-and-credentials)
  - [6.5. Test it !](#65-test-it-)
- [7. Exercice, Rate-Limiting](#7-exercice-rate-limiting)
  - [7.1. Solution](#71-solution)
- [8. Dev Portal](#8-dev-portal)
  - [8.1. Overview](#81-overview)
  - [8.2. Enable it !](#82-enable-it-)
  - [8.3. Add your first spec](#83-add-your-first-spec)
  - [8.4. Test it !](#84-test-it-)
  - [8.5. Excercice](#85-excercice)
    - [8.5.1. Solution](#851-solution)
- [9. Dev Portal, Part two, Authentification](#9-dev-portal-part-two-authentification)
  - [9.1. Enable Basic Auth](#91-enable-basic-auth)
  - [9.2. Limit access](#92-limit-access)
    - [9.2.1. Create a role](#921-create-a-role)
    - [9.2.2. Add role to Spec](#922-add-role-to-spec)
    - [9.2.3. Test it !](#923-test-it-)
      - [9.2.3.1. Register a new developer](#9231-register-a-new-developer)
      - [9.2.3.2. Approve this developer](#9232-approve-this-developer)
      - [9.2.3.3. Add role for this developer](#9233-add-role-for-this-developer)
  - [9.3. Add Oauth2 for developer](#93-add-oauth2-for-developer)
    - [9.3.1. First, remove basic auth](#931-first-remove-basic-auth)
    - [9.3.2. Second, add application-registration plugin](#932-second-add-application-registration-plugin)
    - [9.3.3. Link service and documentation](#933-link-service-and-documentation)
      - [9.3.3.1. Test it !](#9331-test-it-)
- [10. Secure Management Portal](#10-secure-management-portal)
  - [10.1. Create an admin](#101-create-an-admin)
  - [10.2. Enable Basic Auth for Kong Manager](#102-enable-basic-auth-for-kong-manager)
  - [10.3. Use Kong Admin API with RBAC](#103-use-kong-admin-api-with-rbac)
    - [10.3.1. Create and admin user with a token](#1031-create-and-admin-user-with-a-token)

# 2. Introduction

## 2.1. What is IAM ? 

IAM stand for InterSystems API Manager, it's based on **Kong Entreprise Edition**.

This mean you have acces on top of Kong OpenSource edition to :
* Manager Protal
* Developer Portal 
* Advance plugin
  * Oauth2
  * Caching
  * ...

![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/KongEEvsOSS.png)

## 2.2. What is an API Managment ?

API management is the process of creating and publishing web application programming interfaces (APIs), enforcing their usage policies, controlling access, nurturing the subscriber community, collecting and analyzing usage statistics, and reporting on performance. API Management components provide mechanisms and tools to support developer and subscriber community.

![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/Api_Managment.png)

## 2.3. IAM Portal

Kong and IAM are design as API first, this mean, every thing done in Kong/IAM can be done by rest calls or the manager portal.

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

## 2.4. Flow of this training

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
# 3. Installation
## 3.1. What do you need to install? 
* [Git](https://git-scm.com/downloads) 
* [Docker](https://www.docker.com/products/docker-desktop) (if you are using Windows, make sure you set your Docker installation to use "Linux containers").
* [Docker Compose](https://docs.docker.com/compose/install/)
* [Visual Studio Code](https://code.visualstudio.com/download) + [InterSystems ObjectScript VSCode Extension](https://marketplace.visualstudio.com/items?itemName=daimor.vscode-objectscript)
* InterSystems IRIS IAM enabled license file.
* IAM Docker image

## 3.2. How IAM work's with IRIS

At Kong/IAM start, the container check for the Kong/IAM license with a curl call.

The endpoint of this call is a rest API on the IRIS container.

FYI : Kong license is embedded in IRIS one.

![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/IAM_IRIS.png)
## 3.3. Setup

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

## 3.4. Install IAM

### 3.4.1. Iris Image

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

### 3.4.2. IAM Image
In [WRC Software Distribution](https://wrc.intersystems.com/wrc/coDistribution.csp):
* Components > Download *IAM-1.5.0.9-4.tar.gz* file, unzip & untar and then load the image:
```bash
docker load -i iam_image.tar
```

### 3.4.3. Update the docker file

Change IRIS community edition to a licensed one.
* containers.intersystems.com/intersystems/irishealth:2020.4.0.524.0
* add iris.key in key folder

Edit the dockerfile to add on top of it this part
```dockerfile
ARG IMAGE=containers.intersystems.com/intersystems/irishealth:2020.4.0.524.0
# Frist stage
FROM $IMAGE as iris-iam
COPY key/iris.key /usr/irissys/mgr/iris.key
COPY iris-iam.script /tmp/iris-iam.script
RUN iris start IRIS \
&& iris session IRIS < /tmp/iris-iam.script \
&& iris stop IRIS quietly

# Second stage
FROM iris-iam
```

This part will create a multi-stage dockerfile.

* the first stage is to enable IRIS to serve IAM license.
* the second stage is for the REST API build

Create a new file iris-iam.script to build a new IRIS Image to enable IAM endpoint and user.

```objectscript
zn "%SYS"
write "Create web application ...",!
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

### 3.4.4. Update the docker-compose

Update the docker-compose file to :

* db
  * postgres database for IAM
* iam-migration
  * bootstrap the database
* iam
  * actual IAM instance
* a volume for data persistent 

Add this part to the end of the docker-compose file.

```yml
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

Add the .env file in root folder :

```env
IRIS_PASSWORD=SYS
```

### 3.4.5. Option : add IRIS_PASSWARD as .env

For ease of use (and may be security), you can use the .env file in the IRIS dockerfile.

To do so, edit the docker-compose with this in the iris service part :

```yml
    build: 
      context: .
      dockerfile: dockerfile
      args: 
        - IRIS_PASSWORD=${IRIS_PASSWORD}
```

And the dockerfile (second or first stage of the build):

```dockerfile
ARG IRIS_PASSWORD
RUN echo "${IRIS_PASSWORD}" > /tmp/password.txt && /usr/irissys/dev/Container/changePassword.sh /tmp/password.txt
```

### 3.4.6. Test it !

```sh
docker-compose -f "docker-compose.yml" up -d --build
```

# 4. First Service/Route

Remember how Kong/IAM works ?

![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/KongEEvsOSS.png)

Here, we will build :
* a service
  * for our crud API
* a route
  * to acccess this service

## 4.1. Create a service

<table>
<tr>
<th> IAM Portal </th>
<th> Rest API </th>
</tr>
<tr>
<td>

![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/create_service.png)

</td>
<td>

```sh
# Create service

curl -i -X POST \
--url http://localhost:8001/services/ \
--data 'name=crud' \
--data 'url=http://iris:52773/crud/'
```

</td>
</tr>
</table>

What do we see here, to create a service we simply need it's url.

## 4.2. Create a route

<table>
<tr>
<th> IAM Portal </th>
<th> Rest API </th>
</tr>
<tr>
<td>

![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/create_route.png)

</td>
<td>

```sh
# Create route

curl -i -X POST \
--url http://localhost:8001/services/crud/routes \
--data 'name=crud-route' \
--data 'paths=/persons/*' \
--data 'strip_path=false'
```

</td>
</tr>
</table>

What do we see here, to create a route we need :
* it's service name
* a path where RegEx is allowed 

## 4.3. Test it !

<table>
<tr>
<th> Original API </th>
<th> Proxy API </th>
</tr>
<tr>
<td>

```sh
# Legacy


curl –i --location --request GET 'http://localhost:52773/crud/persons/all' \
--header 'Authorization: Basic U3VwZXJVc2VyOlNZUw=='

```

</td>
<td>

```sh
# KONG


curl –i --location --request GET 'http://localhost:8000/persons/all' \
--header 'Authorization: Basic U3VwZXJVc2VyOlNZUw=='

```

</td>
</tr>
</table>

What do we see here :
* Nothing new on legacy side.
* On kong side :
  * We change the port
  * The path correspond to the route
  * We still need to authenticate

# 5. Second, Go futher with plugin

To go futher, we will try to auto-authenticate Kong to the IRIS endpoint.

To do so, we will use and plugin, resquest-transformer.

![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/auto_authenticate.png)

## 5.1. Add a plugin to the service

<table>
<tr>
<th> IAM Portal </th>
<th> Rest API </th>
</tr>
<tr>
<td>

![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/service_plugin.png)

</td>
<td>

```sh
# Create plugin
curl -i -X POST \
--url http://localhost:8001/services/crud/plugins \
--data 'name=request-transformer' \
--data 'config.add.headers=Authorization:Basic U3VwZXJVc2VyOlNZUw==' \
--data 'config.replace.headers=Authorization:Basic U3VwZXJVc2VyOlNZUw=='
```

</td>
</tr>
</table>

## 5.2. Test it !

<table>
<tr>
<th> Original API </th>
<th> Proxy API </th>
</tr>
<tr>
<td>

```sh
# Legacy


curl –i --location --request GET 'http://localhost:52773/crud/persons/all' 


```

</td>
<td>

```sh
# KONG


curl –i --location --request GET 'http://localhost:8000/persons/all' 


```

</td>
</tr>
</table>

What do we see here :
* Error 401 on lecay
* We reatch the data without authentification


# 6. Third, add our own authentication

What we want to achived here is to add our own authentication without any distuption of the orignial API.

![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/custom_auth.png)

## 6.1. Add consumers

<table>
<tr>
<th> IAM Portal </th>
<th> Rest API </th>
</tr>
<tr>
<td>

![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/consumer_anonymous.png)

</td>
<td>

```sh
# Add consumer anonymous
curl -i -X POST \
--url http://localhost:8001/consumers/ \
--data "username=anonymous" \
--data "custom_id=anonymous"
```

</td>
</tr>
<tr>
<td>

![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/consumer_user.png)

</td>
<td>

```sh
# Add consumer user
curl -i -X POST \
--url http://localhost:8001/consumers/ \
--data "username=user" \
--data "custom_id=user"
```

</td>
</tr>
</table>

## 6.2. Add Basic auth plugin

<table>
<tr>
<th> IAM Portal </th>
<th> Rest API </th>
</tr>
<tr>
<td>

![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/basic_auth.png)

</td>
<td>

```sh
# Enable basic auth for service
curl -i -X POST http://localhost:8001/routes/crud-route/plugins \
--data "name=basic-auth" \
--data "config.anonymous=5cc8dee4-066d-492e-b2f8-bd77eb0a4c86" \
--data "config.hide_credentials=false"
```

</td>
</tr>
</table>

Where :
* config.anonymous = uuid of anonymous consumer

## 6.3. Add ACL Plugin

<table>
<tr>
<th> IAM Portal </th>
<th> Rest API </th>
</tr>
<tr>
<td>

![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/acl.png)

</td>
<td>

```sh
# Enable ACL

curl -i -X POST http://localhost:8001/routes/crud-route/plugins \
--data "name=acl" \
--data "config.whitelist=user"
```

</td>
</tr>
</table>

## 6.4. Configure USER with ACL and credentials


<table>
<tr>
<th> IAM Portal </th>
<th> Rest API </th>
</tr>
<tr>
<td>

![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/user_config.png)

</td>
<td>

```sh
# Add consumer group
curl -i -X POST \
--url http://localhost:8001/consumers/user/acls \
--data "group=user"
# Add consumer credentials
curl -i -X POST http://localhost:8001/consumers/user/basic-auth \
--data "username=user" \
--data "password=user"
```

</td>
</tr>
</table>

## 6.5. Test it !

<table>
<tr>
<th> Original API </th>
<th> Proxy API </th>
</tr>
<tr>
<td>

```sh
# Legacy


curl –i --location --request GET 'http://localhost:52773/crud/persons/all' \
--header 'Authorization:Basic dXNlcjp1c2Vy'


```

</td>
<td>

```sh
# KONG

curl –i --location --request GET 'http://localhost:8000/persons/all' \
--header 'Authorization:Basic dXNlcjp1c2Vy'


```

</td>
</tr>
</table>

# 7. Exercice, Rate-Limiting

1. Enable Unauthenticated user
2. Limite rate by 2 calls per minutes to Unauthenticated user

## 7.1. Solution

1. Enable Unauthenticated user

<table>
<tr>
<th> IAM Portal </th>
<th> Rest API </th>
</tr>
<tr>
<td>

![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/acl_anonymous.png)

</td>
<td>

```sh
# Add consumer group

curl -i -X POST \
--url http://localhost:8001/consumers/anonymous/acls \
--data "group=user"
```

</td>
</tr>
</table>

2. Limite rate by 2 calls per minutes to Unauthenticated user

<table>
<tr>
<th> IAM Portal </th>
<th> Rest API </th>
</tr>
<tr>
<td>

![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/rate_limite_user.png)

</td>
<td>

```sh
# Add rate limite consumer
curl -i -X POST \
--url http://localhost:8001/consumers/anonymous/plugins \
--data "name=rate-limiting" \
--data "config.limit_by=consumer" \
--data "config.minute=2"
```

</td>
</tr>
</table>

# 8. Dev Portal

## 8.1. Overview

The Kong Developer Portal provides :
* a single source of truth for all developers
* intuitive content management for documentation
* streamlined developer onboarding
* role-based access control (RBAC)

![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/dev_portal.png)

## 8.2. Enable it !

<table>
<tr>
<th> IAM Portal </th>
<th> Rest API </th>
</tr>
<tr>
<td>

![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/dev_portal_enable.png)

</td>
<td>

```sh
curl -X PATCH http://localhost:8001/workspaces/default --data "config.portal=true"
```

</td>
</tr>
</table>

## 8.3. Add your first spec

<table>
<tr>
<th> IAM Portal </th>
<th> Rest API </th>
</tr>
<tr>
<td>


![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/portal_editor.png)


![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/portal_editor_ui.png)


</td>
<td>

```sh
curl -X POST http://localhost:8001/default/files -F "path=specs/iam-training.yml" -F "contents=@misc/spec.yml"
```

</td>
</tr>
</table>

## 8.4. Test it !

```url
http://localhost:8003/default/documentation/iam-training
```

What happen ? 

How-to solve it ?

## 8.5. Excercice

1. Add CORS plugin on route

### 8.5.1. Solution

<table>
<tr>
<th> IAM Portal </th>
<th> Rest API </th>
</tr>
<tr>
<td>


![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/cors_route.png)


</td>
<td>

```sh
# Enable CORS

curl -i -X POST http://localhost:8001/routes/crud-route/plugins \
--data "name=cors"

```

</td>
</tr>
</table>

# 9. Dev Portal, Part two, Authentification

## 9.1. Enable Basic Auth

<table>
<tr>
<th> IAM Portal </th>
<th> Session Config (JSON) </th>
</tr>
<tr>
<td>


![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/dev_portal_basic.png)


</td>
<td>

```json
{
    "cookie_secure": false,
    "cookie_name": "portal_session",
    "secret": "SYS",
    "storage": "kong"
}
```
</td>
</tr>
</table>

Now authentification is enable for the dev portal.

## 9.2. Limit access

By default, all is accessible for unauthenticated user.

We can create role to limit some access.

For example, let restrict access to our CRUD API documentation.

### 9.2.1. Create a role

<table>
<tr>
<th> IAM Portal </th>
<th> Rest API </th>
</tr>
<tr>
<td>


![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/dev_portal_role.png)


</td>
<td>

```sh
# Enable role

curl -i -X POST http://localhost:8001/default/developers/roles \
--data "name=dev"

```

</td>
</tr>
</table>

### 9.2.2. Add role to Spec

<table>
<tr>
<th> IAM Portal </th>
<th> Rest API </th>
</tr>
<tr>
<td>


![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/dev_portal_acl.png)


</td>
<td>

```sh
# Enable role

curl 'http://localhost:8001/default/files/specs/iam-training.yml' -X PATCH -H 'Accept: application/json, text/plain, */*'  --compressed -H 'Content-Type: application/json;charset=utf-8' -H 'Origin: http://localhost:8002' -H 'Referer: http://localhost:8002/default/portal/permissions/' --data-raw $'{"contents":"x-headmatter:\\n  readable_by:\\n    - dev\\nswagger: \'2.0\'\\ninfo:\\n  title: InterSystems IRIS REST CRUD demo\\n  description: Demo of a simple rest API on IRIS\\n  version: \'0.1\'\\n  contact:\\n    email: apiteam@swagger.io\\n  license:\\n    name: Apache 2.0\\n    url: \'http://www.apache.org/licenses/LICENSE-2.0.html\'\\nhost: \'localhost:8000\'\\nbasePath: /\\nschemes:\\n  - http\\nsecurityDefinitions:\\n  basicAuth:\\n    type: basic\\nsecurity:\\n  - basicAuth: []\\npaths:\\n  /:\\n    get:\\n      description: \' PersonsREST general information \'\\n      summary: \' Server Info \'\\n      operationId: GetInfo\\n      x-ISC_CORS: true\\n      x-ISC_ServiceMethod: GetInfo\\n      responses:\\n        \'200\':\\n          description: (Expected Result)\\n          schema:\\n            type: object\\n            properties:\\n              version:\\n                type: string\\n        default:\\n          description: (Unexpected Error)\\n  /persons/all:\\n    get:\\n      description: \' Retreive all the records of Sample.Person \'\\n      summary: \' Get all records of Person class \'\\n      operationId: GetAllPersons\\n      x-ISC_ServiceMethod: GetAllPersons\\n      responses:\\n        \'200\':\\n          description: (Expected Result)\\n          schema:\\n            type: array\\n            items:\\n              $ref: \'#/definitions/Person\'\\n        default:\\n          description: (Unexpected Error)\\n  \'/persons/{id}\':\\n    get:\\n      description: \' Return one record fo Sample.Person \'\\n      summary: \' GET method to return JSON for a given person id\'\\n      operationId: GetPerson\\n      x-ISC_ServiceMethod: GetPerson\\n      parameters:\\n        - name: id\\n          in: path\\n          required: true\\n          type: string\\n      responses:\\n        \'200\':\\n          description: (Expected Result)\\n          schema:\\n            $ref: \'#/definitions/Person\'\\n        default:\\n          description: (Unexpected Error)\\n    put:\\n      description: \' Update a record in Sample.Person with id \'\\n      summary: \' Update a person with id\'\\n      operationId: UpdatePerson\\n      x-ISC_ServiceMethod: UpdatePerson\\n      parameters:\\n        - name: id\\n          in: path\\n          required: true\\n          type: string\\n        - name: payloadBody\\n          in: body\\n          description: Request body contents\\n          required: false\\n          schema:\\n            type: string\\n      responses:\\n        \'200\':\\n          description: (Expected Result)\\n        default:\\n          description: (Unexpected Error)\\n    delete:\\n      description: \' Delete a record with id in Sample.Person \'\\n      summary: \' Delete a person with id\'\\n      operationId: DeletePerson\\n      x-ISC_ServiceMethod: DeletePerson\\n      parameters:\\n        - name: id\\n          in: path\\n          required: true\\n          type: string\\n      responses:\\n        \'200\':\\n          description: (Expected Result)\\n        default:\\n          description: (Unexpected Error)\\n  /persons/:\\n    post:\\n      description: \' Creates a new Sample.Person record \'\\n      summary: \' Create a person\'\\n      operationId: CreatePerson\\n      x-ISC_ServiceMethod: CreatePerson\\n      parameters:\\n        - name: payloadBody\\n          in: body\\n          description: Request body contents\\n          required: false\\n          schema:\\n            type: string\\n      responses:\\n        \'200\':\\n          description: (Expected Result)\\n        default:\\n          description: (Unexpected Error)\\ndefinitions:\\n  Person:\\n    type: object\\n    properties:\\n      Name:\\n        type: string\\n      Title:\\n        type: string\\n      Company:\\n        type: string\\n      Phone:\\n        type: string\\n      DOB:\\n        type: string\\n        format: date-time\\n"}'

```

</td>
</tr>
</table>

What's important here is this part :

```
contents":"x-headmatter:\\n  readable_by:\\n    - dev
```
Refere to this documentation : 
[readable_by attribute](https://docs.konghq.com/enterprise/1.5.x/developer-portal/administration/developer-permissions/#readable_by-attribute)

### 9.2.3. Test it !

#### 9.2.3.1. Register a new developer

![video](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/video/new_dev_sign.gif)

#### 9.2.3.2. Approve this developer

![video](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/video/approve_new_dev.gif)

#### 9.2.3.3. Add role for this developer

![video](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/video/add_role_dev.gif)

```sh
curl 'http://localhost:8001/default/developers/dev@dev.com' -X PATCH --compressed -H 'Content-Type: application/json;charset=utf-8' -H 'Cache-Control: no-cache' -H 'Origin: http://localhost:8002' -H 'DNT: 1' -H 'Connection: keep-alive' -H 'Referer: http://localhost:8002/default/portal/permissions/dev/update' -H 'Pragma: no-cache' --data-raw '{"roles":["dev"]}'
```

## 9.3. Add Oauth2 for developer

In this part we will add an Oauth2 authentification for developers to use securely our crud API.

This flow will provide sefl regristation from developer and grant them access to the crud API.
### 9.3.1. First, remove basic auth

To do so, we will replace our basic auth to an bearToken one.

First disable our basic auth/acl.

<table>
<tr>
<th> IAM Portal </th>
<th> Rest API </th>
</tr>
<tr>
<td>


![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/acl_disable.png)


</td>
<td>

```sh
# Disable ACL Plugin

curl 'http://localhost:8001/default/routes/afefe836-b9be-49a8-927a-1324a8597a9c/plugins/3f2e605e-9cb6-454a-83ec-d1b1929b1d30' -X PATCH --compressed -H 'Content-Type: application/json;charset=utf-8' -H 'Cache-Control: no-cache' -H 'Origin: http://localhost:8002' -H 'DNT: 1' -H 'Connection: keep-alive' -H 'Referer: http://localhost:8002/default/plugins/acl/3f2e605e-9cb6-454a-83ec-d1b1929b1d30/update' -H 'Pragma: no-cache' --data-raw '{"enabled":false,"name":"acl","route":{"id":"afefe836-b9be-49a8-927a-1324a8597a9c"},"config":{"hide_groups_header":false,"whitelist":["user","dev","crud"]}}'

```

</td>
</tr>
</table>

### 9.3.2. Second, add application-registration plugin

<table>
<tr>
<th> IAM Portal </th>
<th> Rest API </th>
</tr>
<tr>
<td>


![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/application_registration_plugin.png)


</td>
<td>

```sh
# Create application-registration plugin

curl -i -X POST \
--url http://localhost:8001/services/crud/plugins \
--data 'name=application-registration' \
--data 'config.auth_header_name=authorization' \
--data 'config.auto_approve=true' \
--data 'config.display_name=auth' \
--data 'config.enable_client_credentials=true' 
```

</td>
</tr>
</table>

### 9.3.3. Link service and documentation

<table>
<tr>
<th> IAM Portal </th>
<th> Rest API </th>
</tr>
<tr>
<td>


![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/link_service_doc.png)


</td>
<td>

```sh
curl 'http://localhost:8001/default/document_objects' --compressed -H 'Content-Type: application/json;charset=utf-8' -H 'Cache-Control: no-cache' -H 'Origin: http://localhost:8002' -H 'DNT: 1' -H 'Connection: keep-alive' -H 'Referer: http://localhost:8002/default/services/create-documentation' -H 'Pragma: no-cache' --data-raw '{"service":{"id":"7bcef2e6-117c-487a-aab2-c7e57a0bf61a"},"path":"specs/iam-training.yml"}'
```

</td>
</tr>
</table>

#### 9.3.3.1. Test it !

From the dev portal logged as dev@dev.com, create a new application.

![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/video/add_app_dev.gif)

This will give you client_id and client_secret.

Thoses can be used in the swagger dev portal.

Register this application to the crud service :

![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/video/register_app_dev.gif)

Get token:

```sh
curl --insecure -X POST https://localhost:8443/persons/oauth2/token \
  --data "grant_type=client_credentials" \
  --data "client_id=2TXNvDqjeVMHydJbjv9t96lWTXOKAtU8" \
  --data "client_secret=V6Vma6AtIvl04UYssz6gAxPc92eCF4KR"
```

Use token :

```sh
curl --insecure -X GET https://localhost:8443/persons/all \
  --header "authorization: Bearer u5guWaYR3BjZ1KdwuBSC6C7udCYxj5vK"
```

# 10. Secure Management Portal

## 10.1. Create an admin

As we haved bootstrap Kong without a seed password.

We have to create an admin before enforcing RBAC.

To do so: 
* Go to Teams
* Invite admin
  * Set Mail
  * Set Username
    * Set role to super admin
  * Invite
* Go to Invited Admin
* View
* Generate link

![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/video/invite_admin.gif)

## 10.2. Enable Basic Auth for Kong Manager

To enable this feature, we have to change the docker-compose file.

Add this to the iam service, environement

```yml
      KONG_ENFORCE_RBAC: 'on'
      KONG_ADMIN_GUI_AUTH: 'basic-auth'
      KONG_ADMIN_GUI_SESSION_CONF: '{"secret":"${IRIS_PASSWORD}","storage":"kong","cookie_secure":false}'
```

Restart the container

```sh
docker-compose down && docker-compose up -d
```

Go to the invited admin link :

```url
http://localhost:8002/register?email=test.test%40gmail.com&username=admin&token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE2MTYzMzYzNzEsImlkIjoiY2JiZGE5Y2UtODQ3NS00MmM2LTk4ZjItNDgwZTI4MjQ4NWNkIn0.sFeOc_5UPIr3MdlQrgyGvmvIjRFvSn3nQjo2ph8GrJA
```
## 10.3. Use Kong Admin API with RBAC

As RBAC is set, we can't use kong admin api anymore :

```sh
curl -s -X GET \
  --url http://localhost:8001/routes
```

Get this error :

```json
{"message":"Invalid credentials. Token or User credentials required"}
```

### 10.3.1. Create and admin user with a token

* Go to Teams
* RBAC Users
* Add new user

![alt](https://raw.githubusercontent.com/grongierisc/iam-training/training/misc/img/user_rbac.png)


```sh
curl -s -X GET \
  --url http://localhost:8001/routes \
  --header "Kong-Admin-Token: SYS"
```