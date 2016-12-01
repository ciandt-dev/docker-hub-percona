## CI&T Percona (MySQL) Docker image(s)

These Docker image(s) intends to be a containerized Percona solution for multiple purposes.

The source code is available under GPLv3 at Github in this [link]((https://github.com/ciandt-dev/docker-hub-percona).

By utilizing Docker technologies, that already provides an easy way of spinning up new environments along with its dependecies. This image can speed up developers which different backgrounds and equipments to create quickly a new local environment allowing them to easily integrate in automated tests and deployment pipelines.

At this moment we have the following version(s).

### [Acquia](#acquia)

Our intent is to be a Docker container that mimics Percona running on Acquia environment with the same version of softwares, packages, modules and its underlying operating system.

Acquia publishes a table with its platform infrastructure information on the link: https://docs.acquia.com/cloud/arch/tech-platform

These images will have the following name pattern: __acquia-*YYYY-MM-DD*__

#### [*Bundled software versions*](#software-versions)

These are the currently software versions bundled in the image(s) by tag.

* acquia-latest __OR__ acquia-2016-11-30
  * Ubuntu 12.04.5
  * Percona (MySQL) 5.5.24
  * Dumb-init 1.2.0

__*Deprecated*__

* acquia-2016-11-25
* acquia-2016-11-24

## [Quick Start](#quickstart)

__*Download the image*__

```
docker pull ciandtsoftware/percona:acquia-latest
```

__*Run a container*__

```
docker run \
  --name myContainer \
  --detach \
  ciandtsoftware/percona:acquia-latest
```

__*Check running containers*__

```
docker ps --all
```

* * *

## [Running standalone](#running-standalone)

If you just need the container there is a snippet that can help running in standalone mode.

```
# define variables
DOCKER_CONTAINER_NAME="myContainer"
DOCKER_IMAGE="ciandtsoftware/percona:acquia-latest"

# run your container
docker run \
  --name "${DOCKER_CONTAINER_NAME}" \
  --detach \
  "${DOCKER_IMAGE}"
```

After run, you can inquiry Docker service and get the IP address and port of your newly running container named __myContainer__ by using the following command:

```
docker inspect --format '{{ .NetworkSettings.IPAddress }} myContainer'
```

Let's suppose that the returned IP address was __172.17.0.2__.
Since Percona standard port is __3306__, just open a terminal and try to get the stats: access:

```
echo 'SHOW VARIABLES LIKE "%version%";' | mysql --host=172.17.0.2 --user=root --password=admin
```

Percona (MySQL) version information should be returned perfectly.

* * *

## [Running in Docker-Compose](#running-docker-compose)

Since a project is not going to use solely this container, it may need a Docker-Compose file.

Just to exercise, follow an example of this running with __Apache/PHP__.

Create a new folder and a Docker compose file with the content below;

#### [__*docker-compose.yml*__](#docker-compose)

```
percona:
  image: ciandtsoftware/percona:acquia-latest
  container_name: percona
  env_file: ../conf/percona.local.env

php:
  image: ciandtsoftware/php:acquia-latest
  container_name: php
  env_file: ../conf/php.local.env
  links:
    - percona
```

Then just spin-up your Docker-Compose with the command:

```
docker-compose up -d
```

Inspect Apache/PHP container IP address:

```
docker inspect \
        --format \
        "{{.NetworkSettings.Networks.bridge.IPAddress }}" \
        "php"
```

Connect to your Apache/PHP container shell

```
docker exec \
        --interactive \
        --tty \
        php \
        bash
```

Then, try to use MySQL client to connect to your Percona database.

```
mysql \
  --host=percona \
  --user=root \
  --password=admin
```

Voilà!
Your project now have Percona and Apache/PHP up and running.
\\o/

* * *

## [User Feedback](#user-feedback)

### [*Issues*](#issues)

If you have problems, bugs, issues with or questions about this, please reach us in [Github issues page](https://github.com/ciandt-dev/docker-hub-percona/issues).

__Needless to say__, please do a little research before posting.

### [*Contributing*](#contributing)

We gladly invite you to contribute fixes, new features, or updates, large or small; we are always thrilled to receive pull requests, and do our best to process them as fast as we can.

Before you start to code, we recommend discussing your plans through a GitHub issue, especially for more ambitious contributions. This gives other contributors a chance to point you in the right direction, give you feedback on your design, and help you find out if someone else is working on the same thing.

### [*Documentation*](#documentation)

There are __two parts__ of the documentation.

First, in the master branch, is this README.MD. It explains how this little scripts framework work and it is published on [Github page](https://github.com/ciandt-dev/docker-hub-percona).

Second, in each image version there is an additional README.MD file that explains how to use that specific Docker image version itself. __*Latest version*__ is always the one seen on [Docker Hub page](https://hub.docker.com/r/ciandtsoftware/percona).

We strongly encourage reading both!

* * *

Please feel free to drop a message in the comments section.

Happy coding, enjoy!!

"We develop people before we develop software" - Cesar Gon, CEO
