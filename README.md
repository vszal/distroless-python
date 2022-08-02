# distroless-python
TLDR: This repo contains sample apko.yaml files to generate containers for building and running Chainguard's distroless images for python. Please read about distroless at [distroless.dev].

## Overview
This repo contains file to help you build distroless base images for use with python containerized applications. `example/Dockerfile` demostrates a sample multi-stage build (separate builder and run base images). These two base images are generated by [apko](https://github.com/distroless/apko). The builder image which is generated using the `apko-python-builder.yaml` file and the run image which is generated using the `apko-python.yaml` file. Once generated, these two files can be used as base images for your containerized python application.

## Get the apko development image
To create these distroless images you will need the [apko tool](https://github.com/distroless/apko). Follow the instructions to pull the docker image.

## Creating the builder image
1. Use the apko image to build and output the python-builder.tar file:

```
docker run -v $PWD:/work distroless.dev/apko build apko-python-builder.yaml \ 
path/to/your/repo/python-distroless-builder:latest python-builder.tar
```

2. Load the builder image from the tar file:

`docker image load --input python-builder.tar`

3. Push the image to your public/private repo:

`docker push path/to/your/repo/python-distroless-builder:latest`

# Creating the run image
1. Use the apko image to build and output the python.tar file:

```
docker run -v $PWD:/work distroless.dev/apko build apko-python.yaml \
path/to/your/repo/python-distroless:latest python.tar
```

2. Load the builder image from the tar file:

`docker image load --input python.tar`

3. Push the image to your public/private repo:

`docker push path/to/your/repo/python-distroless:latest`

## Feedback
Please send me any feedback and pull requests.
