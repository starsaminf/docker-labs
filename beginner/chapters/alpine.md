## 1.0 Levantando nuestro primer contenedor

Ahora que tienes todo preparado, es hora de ensuciarnos las manos. En esta seccion, vas a ejecutarun [Alpine Linux](http://www.alpinelinux.org/) un contenedor (una distribucion ligera de linux) en tu sistema y pruebar el comando `docker run`.

Para empezar, vamos a correr la siguiente linea en la terminal:
```
$ docker pull alpine
```

> **Nota:** Dependiendo de como instalaste Docker en tu sistema, puede que veas el siguiente mensaje de error `permission denied` despues de correr el comando anterior. Pruebe los comandos del tutorial [verify your installation](https://docs.docker.com/engine/getstarted/step_one/#/step-3-verify-your-installation). Si estas usando Linux, puede que tengas que poner el prefijo `sudo` antes de `docker`. Una alternativa es [crear el grupo docker](https://docs.docker.com/engine/install/linux-postinstall/) para no usar `sudo`.

El comando `pull` busca la **image** alpine en **Docker registry** y lo descarga a en tu sistema. Puedes usar el comando `docker images` para ver una lista de todas las imagenes que descargaste en tu sistema.
```
$ docker images
REPOSITORY              TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
alpine                 latest              c51f86c28340        4 weeks ago         1.109 MB
hello-world             latest              690ed74de00f        5 months ago        960 B
```

### 1.1 Docker Run  
Super! Ahora vamos a correr un **container** basado en la imagen de **alpine**. Para hacer eso vas a usar el siguiente comando `docker run`.

```
$ docker run alpine ls -l
total 48
drwxr-xr-x    2 root     root          4096 Mar  2 16:20 bin
drwxr-xr-x    5 root     root           360 Mar 18 09:47 dev
drwxr-xr-x   13 root     root          4096 Mar 18 09:47 etc
drwxr-xr-x    2 root     root          4096 Mar  2 16:20 home
drwxr-xr-x    5 root     root          4096 Mar  2 16:20 lib
......
......
```
Que paso? Behind the scenes, a lot of stuff happened. When you call `run`,
1. Docker cliente llama al Docker daemon.
2. Docker daemon comprueba si la imagen (alpine en este caso) fue descargada localmente, si no fue descargada, descarga la imagen desde Docker Store.
3. Docker daemon crea el cotenedor y ejecuta el comando.
4. Docker daemon envia la salida del comando al Docker client

Cuando ejecutaste `docker run alpine`, usted proporciono una orden (`ls -l`),  Docker ejecuto el comando y vio la lista.

Intentemos algo mas emocionante.

```
$ docker run alpine echo "hello from alpine"
hello from alpine
```
Esta es la salida del comando. En este caso Docker client ejecuto el comando `echo` en nuestro container alpine.  Si te diste cuenta la ejecucion del comando es muy rapido. Imagina arrancar una maquina virtual, ejecutar el comando y luego destruir la maquina virtual. Ahora sabes por que dicen que los contenedores son muy rapidos.

Intenta con este comando.

```
$ docker run alpine /bin/sh
```

No paso nada! Es un error? Bueno, no. Esta shel termina inmediatamente despues de que se ejecute el comando, a menos que se ejecute en una terminal interactiva, asi que para que no se salga, necesitas correr  `docker run -it alpine /bin/sh`.

Ahora estas dentro el contenedor y puedes probar algunos comandos como `ls -l`, `uname -a` y otros comandos. Pra salir del contenedor ejecuta el comando `exit`.


Ok, Ahora es tiempo de ver el comando `docker ps`. El comando `docker ps` muestra todos los contenedores que estan corriendo actualmente.

```
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

Como no hay contenedores corriendo, ves una linea en blanco. Ahora intenta una variante `docker ps -a`

```
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                      PORTS               NAMES
36171a5da744        alpine              "/bin/sh"                5 minutes ago       Exited (0) 2 minutes ago                        fervent_newton
a6a9d46d0b2f        alpine             "echo 'hello from alp"    6 minutes ago       Exited (0) 6 minutes ago                        lonely_kilby
ff0a5c3750b9        alpine             "ls -l"                   8 minutes ago       Exited (0) 8 minutes ago                        elated_ramanujan
c317d0a9e3d2        hello-world         "/hello"                 34 seconds ago      Exited (0) 12 minutes ago                       stupefied_mcclintock
```

Lo que ves arriba es una lista de todos los contenedores que corriste. Note que la columna `STATUS` muestra que estos contenedores salieron hace unos minutos. Probablemente te estes preguntando si hay una manera de ejecutar mas de un comando en un contendor. Ahora prueba esto:

```
$ docker run -it alpine /bin/sh
/ # ls
bin      dev      etc      home     lib      linuxrc  media    mnt      proc     root     run      sbin     sys      tmp      usr      var
/ # uname -a
Linux 97916e8cb5dc 4.4.27-moby #1 SMP Wed Oct 26 14:01:48 UTC 2016 x86_64 Linux
```
Ejecuta el comando `run` command with the `-it` flags attaches us to an interactive tty in the container. Now you can run as many commands in the container as you want. Take some time to run your favorite commands.

Con esto concluye nuestro tour del comando `docker run` que sera el comando que ejecutara a menudo. Para saber mas sobre el comando `run`, use `docker run --help` para ver una lista de todos los argumentos que soporta. A medida que  avanzas veremos algunas variantes del comando `docker run`.
### 1.2 Terminologia
En esta seccion viste muchos comandos de Docker que podria ser confuso para algunos. Asi que antes de ir mas lejos, Aclaramos algunos terminos que son usados frecuentemente en el ecosistema de Docker.

- *Images* - EL sistema de archivos y las configuraciones de archivos que se usan para crear contenedores. Para saber mas sobre Docker image, prueba este comando `docker inspect alpine`. En la demostracion anterior usaste el comando `docker pull` para descargar la imagen de **alpine**. Cuando ejecutas el comando `docker run hello-world`, tambien se hizo un `docker pull` por detras para descargar la imagen **hello-world** .
- *Containers* - Corriendo una instancia de Docker images &mdash; los contenedores que ejecutan la aplicacion. Un contenedor incluye una aplicacion y todas sus dependencias. Se comparte el kernel con otros contenedores y corre como un proceso aislado. Creaste un contenedor usando el comando `docker run` lo cual hiciste usando la imagen de alpine que descargaste. Se puede ver una lista de contenedores corriendo usando el comando `docker ps`.
- *Docker daemon* - El servicio corriendo por detras en la maquina en el host que gestiona la construccion, el funcionamiento de los contenedores Docker.
- *Docker client* - La herramienta de linea de comandos que permite interactuar al usuario por la linea de comandos con Docker daemon.
- *Docker Store* - Un [registry](https://store.docker.com/) de imagenes Docker donde puedes buscar , where you can find contenedores, plugins y versiones de Docker. Lo usaras mas tarde en este tutorial.

## Next Steps
Para el siguiente tutorial dirijase a [2.0 Webapps with Docker](./webapps.md)
