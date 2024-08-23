<h1> Docker </h1>

Docker **version**
```{bash}
$ docker --version
```
<h2> Image </h2>

List **images** in local repository

```{bash}
$ docker image ls
```

Build Docker **image from Dockerfile** located at current directory (must be named Dockerfile)

```{bash}
$ docker build -t 'název image' .
```
**Remove** image. !!! You cannot simply remove image which container is used (exited state)

```{bash}
$ docker image rm <image_id>
```

<h2> Container </h2>

**Run** image as container (must be presented in local registry). 

```{bash}
$ docker run --name 'název kontajneru' -p 8080:80 'název image'
```

**Remove** container

```{bash}
$ docker rm <container_id_or_name>
```

**Stop** running container

```{bash}
$ docker stop <container id or name>
```

**Start** container

```{bash}
$ docker start <container id or name>
```
**Restart** container

```{bash}
$ docker restart <container id or name>
```

List **running containers**

```{bash}
$ docker ps
```
List **all containers**

```{bash}
$ docker ps --all
```

<h3> Debug </h3>

**Logy** z kontajneru (parametr -f v reálném čase)

```{bash}
$ docker logs <container id or name>
```

**Výpis procesů** z kontajneru (Network -> nastaveno na 'bridge' + IP (IP pro bridge)

```{bash}
$ docker inspect <container id or name>
```

**Napojení terminálu** do kontajneru (shell)

```{bash}
$ docker exec -ti <container id or name> sh 
```

<h2> Volume </h2>

**Vytvoření** nového volume

```{bash}
$ docker volume create 'název' 
```
**Výpis** volume

```{bash}
$ docker volume ls
```

<h2> Network </h2>

**Vytvoření** nové network

```{bash}
$ docker network create 'název' 
```
**Výpis** network

```{bash}
$ docker network ls
```

- --rm - po ukončení kontejneru zanikne
- -p - mapování portů, aby PC byl na stejném portu jako docker ( -p 8888:8888)
- -t - tag (odlišení verze/změn): Docker Wordpress:1 -> název image + verze
- -d - poběží na pozadí, nebudou vidět logy, pro více kontajnerů
- cat - otevře TXT soubor
- tail -f  - stále dokola se bude číst daný soubor + zobrazovat (musíme vědět ze soubo bude existovat) 
- -f - logy se budou stále updatovat (v reálném čase)
-  mount source='název volume' - specifikace volume
- target - konrétní cílová složka pro volume (nedoporučuje se to dávat na root)
- --net 'název networku' - specifikujeme připojení jiného networku 
