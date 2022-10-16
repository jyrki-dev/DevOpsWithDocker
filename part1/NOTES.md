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

To launch a container detached from the shell, use `docker container run -d`, leaving the container running in the background.

List all running containers with `docker container ls`.

You have to stop a container with `docker containder stop <id>` before being able to remove it.
