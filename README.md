## dezoomcampcodespace2024
_This repo is to learn about github developement-environment-as-service --> **_codespace_** and run notebook,docker container in codespace_

## What is [github codespace](https://github.com/features/codespaces)

* Github codespace is a fully configured dev environment in cloud that starts in second with upto 60 hrs in month free.
* All you need to free github account to start using codespace.
* With free account we have access to 2-core(4gb ram,60 hr fee /month) and 4-core(8gb ram, 30 hrs free /month) option.
Please follow github documentation on [codespace][def11] for further informations.

**Lets get started**

## Initialize code space

* Create a free [github](https://github.com/) account
* Create a github repo
* Go to your Github repo page and click on Code drop down on top right
  
  ![Alt text][def2]
* Click on + sign to create new Codespace environment (_default is 2 core 4 gb_) on your repo
* It will spin up a Codespace environment with a unique name and open the VS code alike web interface
  
  ![Alt text][def3]
* Click the hamburger button on top left and open Codespace in VS Code for Desktop
  
  ![Alt text][def4]
* It will install the same in local machine/vm if not installed
  
  ![Alt text][def5]
* Remote Codespace Dev environment(Compute+Storage) from VS Code with Git repo initialized
  
  ![Alt text][def6]

* Close the web interface of Codespace
## Check what comes pre installed in the environment

```sh
$ python --version
```
```sh
$ pip list
```
```sh
$ docker version
```
## Install and check jupyter notebook

```sh
$ pip install jupyter
```
* Open jupyter notebook
  ```sh
  $ jupyter notebook
  ```
* Check port tab in vs-code

![Alt text][def7]
* Open localhost:8888 in browser.It will open the workspace in jupyter notebook environment
  
  ![Alt text][def8]

* Execute python statement in jupyter notebook

![Alt text][def9]

## docker setup for postgres container

* docker command to create a virtual network

```bash
$ docker network create pg-network
```
* docker command to list networks
  
```bash
$ docker network ls
```
![Alt text][def]

* docker local volume creation to mount postgress volume
```sh
$ docker volume create --name dtc_postgress_volume_local -d local
```
* check docker volume list
```sh
$ docker volume ls
```
* Run a docker postgres  container in newly created environment
```sh
$ docker run -it \
>   -e POSTGRES_USER="root" \
>   -e POSTGRES_PASSWORD="root" \
>   -e POSTGRES_DB="ny_taxi" \
>   -v dtc_postgress_volume_local:/var/lib/postgresql/data \
>   -p 5432:5432 \
>   --network=pg-network \
>   --name pg-database \
> postgres:13
```
## pgcli setup to communicate with postgres db ny_taxi

* Add another bash terminal. Install and run pgcli on localhost to check the database 'ny_taxi'
```sh
$ pip install pgcli
```
```sh
 $ pgcli -h localhost -p 5432 -u root -d ny_taxi -W
```
* As there are no tables yet \d (Describe) will return empty list in ny_taxi db
  
  ![Alt text][def10]

## docker setup to run pgadmin container(communication between two docker container(postgres & pgadmin) under same network )
* docker local volume creation to mount pgadmin volume
```sh
$ docker volume create --name dtc_pgadmin_volume_local -d local
```
* check docker volume list
```sh
$ docker volume ls
```
* Make pgAdmin configuration persistent.change local volume permission

* Run a docker postgres  container in newly created environment
  
```sh
docker run -it \
    -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" \
    -e PGADMIN_DEFAULT_PASSWORD="root" \
    -v dtc_pgadmin_volume_local:/var/lib/postgresql/data:rw \
    -p 8080:80 \
    --network=pg-network \
    --name pgadmin \
dpage/pgadmin4
```
* Stop both postgres  and pgadmin container by press ctrl+c(windows)

[def]: ./Images/image.png
[def2]: ./Images/image1.png
[def3]: ./Images/image3.png
[def4]: /Images/image4.png
[def5]: ./Images/image5.png
[def6]: ./Images/image6.png
[def7]: ./Images/image7.png
[def8]: ./Images/image8.png
[def9]: ./Images/image9.png
[def10]: ./Images/image10.png
[def11]: https://docs.github.com/en/codespaces/overview

