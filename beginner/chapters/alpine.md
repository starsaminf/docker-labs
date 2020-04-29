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
OK, that's some actual output. In this case, the Docker client dutifully ran the `echo` command in our alpine container and then exited it. If you've noticed, all of that happened pretty quickly. Imagine booting up a virtual machine, running a command and then killing it. Now you know why they say containers are fast!

Try another command.
```
$ docker run alpine /bin/sh
```

Wait, nothing happened! Is that a bug? Well, no. These interactive shells will exit after running any scripted commands, unless they are run in an interactive terminal - so for this example to not exit, you need to `docker run -it alpine /bin/sh`.

You are now inside the container shell and you can try out a few commands like `ls -l`, `uname -a` and others. Exit out of the container by giving the `exit` command.


Ok, now it's time to see the `docker ps` command. The `docker ps` command shows you all containers that are currently running.

```
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

Since no containers are running, you see a blank line. Let's try a more useful variant: `docker ps -a`

```
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                      PORTS               NAMES
36171a5da744        alpine              "/bin/sh"                5 minutes ago       Exited (0) 2 minutes ago                        fervent_newton
a6a9d46d0b2f        alpine             "echo 'hello from alp"    6 minutes ago       Exited (0) 6 minutes ago                        lonely_kilby
ff0a5c3750b9        alpine             "ls -l"                   8 minutes ago       Exited (0) 8 minutes ago                        elated_ramanujan
c317d0a9e3d2        hello-world         "/hello"                 34 seconds ago      Exited (0) 12 minutes ago                       stupefied_mcclintock
```

What you see above is a list of all containers that you ran. Notice that the `STATUS` column shows that these containers exited a few minutes ago. You're probably wondering if there is a way to run more than just one command in a container. Let's try that now:

```
$ docker run -it alpine /bin/sh
/ # ls
bin      dev      etc      home     lib      linuxrc  media    mnt      proc     root     run      sbin     sys      tmp      usr      var
/ # uname -a
Linux 97916e8cb5dc 4.4.27-moby #1 SMP Wed Oct 26 14:01:48 UTC 2016 x86_64 Linux
```
Running the `run` command with the `-it` flags attaches us to an interactive tty in the container. Now you can run as many commands in the container as you want. Take some time to run your favorite commands.

That concludes a whirlwind tour of the `docker run` command which would most likely be the command you'll use most often. It makes sense to spend some time getting comfortable with it. To find out more about `run`, use `docker run --help` to see a list of all flags it supports. As you proceed further, we'll see a few more variants of `docker run`.
### 1.2 Terminology
In the last section, you saw a lot of Docker-specific jargon which might be confusing to some. So before you go further, let's clarify some terminology that is used frequently in the Docker ecosystem.

- *Images* - The file system and configuration of our application which are used to create containers. To find out more about a Docker image, run `docker inspect alpine`. In the demo above, you used the `docker pull` command to download the **alpine** image. When you executed the command `docker run hello-world`, it also did a `docker pull` behind the scenes to download the **hello-world** image.
- *Containers* - Running instances of Docker images &mdash; containers run the actual applications. A container includes an application and all of its dependencies. It shares the kernel with other containers, and runs as an isolated process in user space on the host OS. You created a container using `docker run` which you did using the alpine image that you downloaded. A list of running containers can be seen using the `docker ps` command.
- *Docker daemon* - The background service running on the host that manages building, running and distributing Docker containers.
- *Docker client* - The command line tool that allows the user to interact with the Docker daemon.
- *Docker Store* - A [registry](https://store.docker.com/) of Docker images, where you can find trusted and enterprise ready containers, plugins, and Docker editions. You'll be using this later in this tutorial.

## Next Steps
For the next step in the tutorial, head over to [2.0 Webapps with Docker](./webapps.md)
