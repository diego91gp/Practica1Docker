# PRACTICA  DOCKER DIEGO GUTIERREZ PILAR

[TOC]

### 1.Instala docker en una máquina y configúralo para que se pueda usar con un usuario sin privilegios.

Instalamos docker

```bash
daw@daw-docker:~$ sudo apt install docker.io
```

Configuramos docker para que lo pueda usar unusuario sin privilegios  con el siguiente comando.

```bash
daw@daw-docker:~$ sudo usermod -aG docker usuario2
```

### 2.Ejecuta un contenedor a partir de la imagen hello-word.

```bash
usuario2@daw-docker:~$ docker run --name holaMundo hello-world
```

#### a)Comprueba que nos devuelve la salida adecuada.

![](.\imagenes\Captura1.png)

#### b) Comprueba que no se está ejecutando. 

![](.\imagenes\Captura3.png)

#### c)Lista los contenedores que están parados.

![](.\imagenes\Captura2.png)

####  d)Borra el contenedor.

Con el comando:

```bash
usuario2@daw-docker:~$ docker rm holaMundo 
```

![](.\imagenes\Captura4.png)

### 3. Crea un contenedor interactivo desde una imagen debian. 

```bash
usuario2@daw-docker:~$ docker run -it --name Debian debian
Unable to find image 'debian:latest' locally
latest: Pulling from library/debian
bbeef03cda1f: Pull complete 
Digest: sha256:534da5794e770279c889daa891f46f5a530b0c5de8bfbc5e40394a0164d9fa87
Status: Downloaded newer image for debian:latest
root@39bc18a65078:/#
```

#### a)Instala un paquete (por ejemplo nano). 

```bash
root@39bc18a65078:/# apt install nano
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  libgpm2 libncursesw6
Suggested packages:
  gpm hunspell
The following NEW packages will be installed:
  libgpm2 libncursesw6 nano
0 upgraded, 3 newly installed, 0 to remove and 0 not upgraded.
Need to get 825 kB of archives.
After this operation, 3087 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://deb.debian.org/debian bullseye/main amd64 libncursesw6 amd64 6.2+20201114-2 [132 kB]
Get:2 http://deb.debian.org/debian bullseye/main amd64 nano amd64 5.4-2+deb11u2 [657 kB]
Get:3 http://deb.debian.org/debian bullseye/main amd64 libgpm2 amd64 1.20.7-8 [35.6 kB]
Fetched 825 kB in 0s (5046 kB/s)   
debconf: delaying package configuration, since apt-utils is not installed
Selecting previously unselected package libncursesw6:amd64.
(Reading database ... 6661 files and directories currently installed.)
Preparing to unpack .../libncursesw6_6.2+20201114-2_amd64.deb ...
Unpacking libncursesw6:amd64 (6.2+20201114-2) ...
Selecting previously unselected package nano.
Preparing to unpack .../nano_5.4-2+deb11u2_amd64.deb ...
Unpacking nano (5.4-2+deb11u2) ...
Selecting previously unselected package libgpm2:amd64.
Preparing to unpack .../libgpm2_1.20.7-8_amd64.deb ...
Unpacking libgpm2:amd64 (1.20.7-8) ...
Setting up libgpm2:amd64 (1.20.7-8) ...
Setting up libncursesw6:amd64 (6.2+20201114-2) ...
Setting up nano (5.4-2+deb11u2) ...
update-alternatives: using /bin/nano to provide /usr/bin/editor (editor) in auto mode
update-alternatives: using /bin/nano to provide /usr/bin/pico (pico) in auto mode
Processing triggers for libc-bin (2.31-13+deb11u5) ...
```

#### b)Sal de la terminal, ¿sigue el contenedor corriendo? ¿Por qué?. 

No sigue corriendo, porque se cerró y no esta corriendo en segundo plano.

![](.\imagenes\Captura5.png)

#### c)Vuelve a iniciar el contenedor y accede de nuevo a él de forma interactiva. ¿Sigue instalado el nano?. 

Sigue instalado.

![](.\imagenes\Captura6.png)

#### d)Sal del contenedor, y bórralo. 

```bash
root@39bc18a65078:/# exit
usuario2@daw-docker:~$ docker rm Debian
```

#### e)Crea un nuevo contenedor interactivo desde la misma imagen. ¿Tiene el nano instalado?

No esta instalado.

```bash
usuario2@daw-docker:~$ docker run -it --name Debian2 debian
root@75323ab18a3e:/# nano --version
bash: nano: command not found
root@75323ab18a3e:/# 
```

### 4.Crea un contenedor demonio con un servidor nginx, usando la imagen oficial de nginx. 

```bash
usuario2@daw-docker:~$ docker run -d --name nginx nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
```

![](.\imagenes\Captura7.png)

#### a)Al crear el contenedor, ¿has tenido que indicar algún comando para que lo ejecute? 

-d Para que corra en segundo plano.

#### Accede al navegador web y comprueba que el servidor esta funcionando. 

![](.\imagenes\Captura8.png)

#### b)Muestra los logs del contenedor.

```bash
usuario2@daw-docker:~$ docker logs nginx
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2023/01/15 08:10:18 [notice] 1#1: using the "epoll" event method
2023/01/15 08:10:18 [notice] 1#1: nginx/1.23.3
2023/01/15 08:10:18 [notice] 1#1: built by gcc 10.2.1 20210110 (Debian 10.2.1-6) 
2023/01/15 08:10:18 [notice] 1#1: OS: Linux 5.15.0-57-generic
2023/01/15 08:10:18 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2023/01/15 08:10:18 [notice] 1#1: start worker processes
2023/01/15 08:10:18 [notice] 1#1: start worker process 28
2023/01/15 08:10:18 [notice] 1#1: start worker process 29
172.17.0.1 - - [15/Jan/2023:08:21:53 +0000] "GET / HTTP/1.1" 200 615 "-" "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:108.0) Gecko/20100101 Firefox/108.0" "-"
172.17.0.1 - - [15/Jan/2023:08:21:53 +0000] "GET /favicon.ico HTTP/1.1" 404 153 "http://172.17.0.2/" "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:108.0) Gecko/20100101 Firefox/108.0" "-"
2023/01/15 08:21:53 [error] 28#28: *1 open() "/usr/share/nginx/html/favicon.ico" failed (2: No such file or directory), client: 172.17.0.1, server: localhost, request: "GET /favicon.ico HTTP/1.1", host: "172.17.0.2", referrer: "http://172.17.0.2/"
```

### 5.Crea un contenedor con la aplicación Nextcloud, mirando la documentación en docker Hub, para personalizar el nombre de la base de datos sqlite que va a utilizar.



```bash
usuario2@daw-docker:~$ docker run -d -p 8080:80 --name nextCloud -e SQLITE_DATABASE=myBase nextcloud
Unable to find image 'nextcloud:latest' locally
latest: Pulling from library/nextcloud
8740c948ffd4: Already exists 
```

![](.\imagenes\Captura9.png)

## -*Webgrafía y materiales usados-*

Se han usado los siguientes recursos:
Documentacion sobre nextCloud en dockerhub
https://hub.docker.com/_/nextcloud
