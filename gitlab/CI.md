
### Docker

##### Use Docker-in-Docker workflow with Docker executor

The second approach is to use the special Docker-in-Docker (dind) Docker image with all tools installed (docker) and run the job script in context of that image in privileged mode.

docker-compose is not part of Docker-in-Docker (dind). To use docker-compose in your CI builds, follow the docker-compose installation instructions.
Warning: By enabling --docker-privileged, you are effectively disabling all of the security mechanisms of containers and exposing your host to privilege escalation which can lead to container breakout. For more information, check out the official Docker documentation on Runtime privilege and Linux capabilities.

Docker-in-Docker works well, and is the recommended configuration, but it is not without its own challenges:

1.    When using Docker-in-Docker, each job is in a clean environment without the past history. Concurrent jobs work fine because every build gets its own instance of Docker engine so they don’t conflict with each other. But this also means that jobs can be slower because there’s no caching of layers.
    By default, Docker 17.09 and higher uses --storage-driver overlay2 which is the recommended storage driver. See Using the overlayfs driver for details.

2.    Since the docker:19.03.12-dind container and the runner container don’t share their root file system, the job’s working directory can be used as a mount point for child containers. For example, if you have files you want to share with a child container, you may create a subdirectory under /builds/$CI_PROJECT_PATH and use it as your mount point (for a more thorough explanation, check issue #41227):
```
    variables:
      MOUNT_POINT: /builds/$CI_PROJECT_PATH/mnt

    script:
      - mkdir -p "$MOUNT_POINT"
      - docker run -v "$MOUNT_POINT:/mnt" my-docker-image
```

An example project using this approach can be found here: https://gitlab.com/gitlab-examples/docker.

In the examples below, we are using Docker images tags to specify a specific version, such as docker:19.03.12. If tags like docker:stable are used, you have no control over what version is used. This can lead to unpredictable behavior, especially when new versions are released.
TLS enabled

The Docker daemon supports connection over TLS and it’s done by default for Docker 19.03.12 or higher. This is the suggested way to use the Docker-in-Docker service and GitLab.com shared runners support this.
Docker

Introduced in GitLab Runner 11.11.

1.    Install [GitLab Runner](https://docs.gitlab.com/runner/install/).

2.    Register GitLab Runner from the command line to use docker and privileged mode:
```
    sudo gitlab-runner register -n \
      --url http://192.168.9.189/ \
      --registration-token "jo3Sp--rn_h2V6LNW7Pr" \
      --executor docker \
      --description "My Docker Runner" \
      --docker-image "docker:19.03.13" \
      --docker-privileged \
      --docker-volumes "/certs/client"
```
The above command registers a new runner to use the special docker:19.03.12 image, which is provided by Docker. Notice that it’s using the privileged mode to start the build and service containers. If you want to use Docker-in-Docker mode, you always have to use privileged = true in your Docker containers.

This also mounts /certs/client for the service and build container, which is needed for the Docker client to use the certificates inside of that directory. For more information on how Docker with TLS works, check https://hub.docker.com/_/docker/#tls.

The above command creates a config.toml entry similar to this:
```
[[runners]]
  url = "https://gitlab.com/"
  token = TOKEN
  executor = "docker"
  [runners.docker]
    tls_verify = false
    image = "docker:19.03.12"
    privileged = true
    disable_cache = false
    volumes = ["/certs/client", "/cache"]
  [runners.cache]
    [runners.cache.s3]
    [runners.cache.gcs]
```
You can now use docker in the build script (note the inclusion of the docker:19.03.12-dind service):
```
image: docker:19.03.13

variables:
  # When using dind service, we need to instruct docker to talk with
  # the daemon started inside of the service. The daemon is available
  # with a network connection instead of the default
  # /var/run/docker.sock socket. Docker 19.03 does this automatically
  # by setting the DOCKER_HOST in
  # https://github.com/docker-library/docker/blob/d45051476babc297257df490d22cbd806f1b11e4/19.03/docker-entrypoint.sh#L23-L29
  #
  # The 'docker' hostname is the alias of the service container as described at
  # https://docs.gitlab.com/ee/ci/docker/using_docker_images.html#accessing-the-services.
  #
  # Specify to Docker where to create the certificates, Docker will
  # create them automatically on boot, and will create
  # `/certs/client` that will be shared between the service and job
  # container, thanks to volume mount from config.toml
  DOCKER_TLS_CERTDIR: "/certs"

services:
  - docker:19.03.12-dind

before_script:
  - docker info

build:
  stage: build
  script:
    - docker build -t my-docker-image .
    - docker run my-docker-image /script/to/run/tests
```
Kubernetes

Introduced in GitLab Runner Helm Chart 0.23.0.

Using the Helm chart, update the values.yml file to specify a volume mount.
```
    runners:
      config: |
        [[runners]]
          [runners.kubernetes]
            image = "ubuntu:20.04"
            privileged = true
          [[runners.kubernetes.volumes.empty_dir]]
            name = "docker-certs"
            mount_path = "/certs/client"
            medium = "Memory"
````
You can now use docker in the build script (note the inclusion of the docker:19.03.13-dind service):
```
image: docker:19.03.13

variables:
  # When using dind service, we need to instruct docker to talk with
  # the daemon started inside of the service. The daemon is available
  # with a network connection instead of the default
  # /var/run/docker.sock socket.
  DOCKER_HOST: tcp://docker:2376
  #
  # The 'docker' hostname is the alias of the service container as described at
  # https://docs.gitlab.com/ee/ci/docker/using_docker_images.html#accessing-the-services.
  # If you're using GitLab Runner 12.7 or earlier with the Kubernetes executor and Kubernetes 1.6 or earlier,
  # the variable must be set to tcp://localhost:2376 because of how the
  # Kubernetes executor connects services to the job container
  # DOCKER_HOST: tcp://localhost:2376
  #
  # Specify to Docker where to create the certificates, Docker will
  # create them automatically on boot, and will create
  # `/certs/client` that will be shared between the service and job
  # container, thanks to volume mount from config.toml
  DOCKER_TLS_CERTDIR: "/certs"
  # These are usually specified by the entrypoint, however the
  # Kubernetes executor doesn't run entrypoints
  # https://gitlab.com/gitlab-org/gitlab-runner/-/issues/4125
  DOCKER_TLS_VERIFY: 1
  DOCKER_CERT_PATH: "$DOCKER_TLS_CERTDIR/client"

services:
  - docker:19.03.13-dind

before_script:
  - docker info

build:
  stage: build
  script:
    - docker build -t my-docker-image .
    - docker run my-docker-image /script/to/run/tests
```
