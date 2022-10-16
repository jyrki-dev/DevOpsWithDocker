# DevOps With Docker - Part 1

## Workflow

1. Create a Dockerfile
2. Build an image from it
    2.1. Host the image to a repository
3. Run the image

## Basic commands

| Command                                | Functionality                           | Shorthand
|----------------------------------------|-----------------------------------------|--------------
| `docker image ls`                      | Lists all images                        | docker images
| `docker image rm <image>`              | Removes an image                        | docker rmi
| `docker image pull <image>`            | Pulls image from a docker registry      | docker pull
| `docker container ls -a`               | Lists all containers                    | docker ps -a
| `docker container run <image>`         | Runs a container from an image          | docker run
| `docker container rm <container>`      | Removes a container                     | docker rm
| `docker container stop <container>`    | Stops a container                       | docker stop
| `docker container exec <container>`    | Executes a command inside the container | docker exec
| `docker build image`                   | Builds an image from a Dockerfile       | docker build help
| `docker container prune`               | Removes all stopped containers          | dcpr
| `docker image prune`                   | Removes all 'dangling' images           | docker image prune
| `docker system prune`                  | Removes almost everything               | docker system prune

## Running containers

To launch a container detached from the shell, use `docker container run -d`, leaving the container running in the background. You can reattach the detached container with `docker attach <id>`. If you want to ensure `<Ctrl-C>` wont stop the container, use the `--no-stdin` flag. Then `<Ctrl-C>` only disconnect your from the containers `STDOUT`.

List all running containers with `docker container ls`.

To remove a container, either run `docker [container] rm <id>`, or let Docker autoclean itself with `docker container prune`. You can also provide `docker run` with the flag `--rm`, which automatically removes the container on exit.

You have to stop a container with `docker containder stop <id>` before being able to remove it.
If the container is running a process that wont listen to a SIGTERM signal, you can stop it by sending it a SIGKILL signal with `docker kill <id>` (docker will automatically send it after a grace period if `stop` doesn't work).

To launch interact with the container, you need to pass the `-t / --tty` and `-i / --interactive` flags: `docker run -it ubuntu`

The `-t` flag creates a pseudo-tty, but it needs to be made interactive with the `-i` flag to make your shell pass STDIN to the container.

To see the output of logs of a container, run `docker logs -f looper` (the `-f / --follow` flag allows you to monitor the log output in realtime).

## Dockerfile

Images are constructed from `Dockerfiles`, which are a series of instrunctions and commands that are run in sequence.

### ENTRYPOINT vs CMD

For passing executable commands to a container, there are two main fields in a Dockerfile: `ENTRYPOINT` and `CMD`.`ENTRYPOINT` is used to define the main executable (think of it as a prefix for the command to run), while `CMD` is the default command that is run when giving no argument to a container.

There are two ways to set both: **exec** form and **shell** form. In exec form, the command itself is executed directly, and in shell form it's wrapped with `/bin/sh -c`. Shell form is especially useful when needing to evalueate environment variables.

In the shell form the command is provided as a string without brackets. In the exec form the command and it's arguments are provided as a list (with brackets).

| Dockerfile                                                   | Resulting Command
|--------------------------------------------------------------|----------------------------------------------------|
| `ENTRYPOINT /bin/ping -c 3`<br/>`CMD localhost`              | `/bin/sh -c '/bin/ping -c 3' /bin/sh -c localhost` |
| `ENTRYPOINT ["/bin/ping","-c","3"]`<br/>`CMD localhost`      | `/bin/ping -c 3 /bin/sh -c localhost`              |
| `ENTRYPOINT /bin/ping -c 3;`<br/>`CMD ["localhost"]`         | `/bin/sh -c '/bin/ping -c 3' localhost`            |
| `ENTRYPOINT ["/bin/ping","-c","3"]`<br/>`CMD ["localhost"]`  | `/bin/ping -c 3 localhost`                         |

### Building images

When you've created a `Dockerfile`, run `docker build <directory> -t <name>`, and Docker will start the build process. The `-t / --tag` flag allows you to name the image with the `image:tag` syntax.

The build process creates multiple layers and intermediate containers during the build process. Layers can work as a cache during build time. If we just edit the last lines of Dockerfile the build command can start from the previous layer and skip straight to the section that has changed.

The intermediate containers are containers created from the image in which the command is executed. Then the changed state is stored into an image.

### Operating inside containers

You can copy files from your local filesystem to a running container with the command `docker cp <local-file-path> <container>:<directory>`.

You can view the diff of an altered container with `docker diff <container>`.

To save the made changes (to a new image), run `docker commit <container> <image>`.
