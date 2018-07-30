# Ambiente de Integración Continua

Este proyecto busca orientar sobre cómo crear un ambiente de integración continua usando [Rancher](https://rancher.com/), [Gitlab](https://about.gitlab.com/) y [Prometheus](https://prometheus.io/).

## 1. Creación de rancher

  Para crear un ambiente funcional de [Rancher](https://rancher.com/) usando [Vagrant](https://vagrantup.com) siga este [**tutorial**](https://github.com/tutmosisII/vagrant_rancher)

## 2. Gitlab

En su útimas versiones este proyecto se ha convertido en un poderoso motor de Integración continua (CI) que contiene las siguientes herramientas.
  * [Docker](https://www.docker.com/) Registry
  * [Prometheus](https://prometheus.io/) para monitoreo
  * [Mattermost](https://mattermost.com/) Para Comunicación entre equipos.
  * API para integración con terceros.

### 2.1 Desplegando Gitlab por medio de Rancher.

Para iniciar vaya a la cerpeta [node_gitlab](node_gitlab) y ejecute el siguiente comando:

    vagrant up

Cuando el nodo este arriba agregeló a Rancher siguiendo los pasos indicados en [**tutorial rancher**](https://github.com/tutmosisII/vagrant_rancher)

Para ingresar el nodeo por ssh use este comando:

    vagrant ssh gitlab

Gitlab usa la ip 192.168.1.102.Pegue el comando obtenido de Rancher.

Ahora Rancher en la sección de Host debería verse así:

![Nodos Rancher](images/rancher_nodes.png)
#### 2.1.1 Desplegando Gitlab

>##### Image
1.Cree un stack llamado gitlab.

>2.Adicione un servicio al stack llamelo gitlab
>
>3.Image: gitlab/gitlab-ce:latest

![paso](images/rancher_s01.png)


> ##### Volumes:
> En la pestaña de **volumenes** configure los siguientes.

>  1. gitlab-etc:/etc/gitlab
>
>  2. gitlab-opt:/var/opt/gitlab
>
>  3. gitlab-log:/var/log/gitlab

![paso](images/rancher_s02.png)

> ##### Networking
> En la pestañana de **networking** configure lo siguiente
Configure el nombre del host: git
![paso](images/rancher_s03.png)

>
> ##### Health Check
>
HTTP: Port 80
Path: GET / HTTP/1.0
>
![paso](images/rancher_s04.png)

> Deje los otros valores por defecto.

***

> ##### Sidekick
>Add a sidekick to the service called **postfix**
>
Image: tozd/postfix

![paso](images/rancher_s05.png)


>##### Environment (postfix):
>
MY_NETWORKS:10.42.0.0/16, 127.0.0.0/8
>
ROOT_ALIAS: you@localhost

![paso](images/rancher_s06.png)

>##### Volumes (postfix):
postfix-log:/var/log/postfix
>
postfix-spool:/var/spool/postfix
>
![paso](images/rancher_s07.png)

>##### Health Check (postfix):
TCP: Port 25

![paso](images/rancher_s08.png)

### 2.1.2 Configurando SSL

La configuración require un DNS (bind), un balanceador de Carga y Let's encrypt.

## Requerimientos

Rancher: 1GB

Gitlab:  4GB, 2 cup

## Referencias

Buena parte de esta documentación esta basada en:
https://rancher.com/how-to-run-gitlab-in-rancher-1/

Servidor DNS local http://www.damagehead.com/blog/2015/04/28/deploying-a-dns-server-using-docker/
https://github.com/sameersbn/docker-bind
