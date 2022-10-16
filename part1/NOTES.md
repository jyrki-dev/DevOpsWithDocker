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

To launch interact with the container, you need to pass the `-t / --tty` and `-i / --interactive` flags:
> `docker run -it ubuntu`

The `-t` flag creates a pseudo-tty, but it needs to be made interactive with the `-i` flag to make your shell pass STDIN to the container.

To see the output of logs of a container, run `docker logs -f looper` (the `-f / --follow` flag allows you to monitor the log output in realtime).