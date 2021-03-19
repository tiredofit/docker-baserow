# hub.docker.com/r/tiredofit/baserow

[![Docker Pulls](https://img.shields.io/docker/pulls/tiredofit/baserow.svg)](https://hub.docker.com/r/tiredofit/baserow)
[![Docker Stars](https://img.shields.io/docker/stars/tiredofit/baserow.svg)](https://hub.docker.com/r/tiredofit/baserow)
[![Docker Layers](https://images.microbadger.com/badges/image/tiredofit/baserow.svg)](https://microbadger.com/images/tiredofit/baserow)

## Introduction

This will build a container for [Baserow](https://baserow.io/) - An open source low/no code data management solution

* Automatically installs and sets up installation upon first start

* This Container uses a [customized Alpine base](https://hub.docker.com/r/tiredofit/alpine) which includes [s6 overlay](https://github.com/just-containers/s6-overlay) enabled for PID 1 Init capabilities, [zabbix-agent](https://zabbix.org) for individual container monitoring, Cron also installed along with other tools (bash,curl, less, logrotate, nano, vim) for easier management. It also supports sending to external SMTP servers..

[Changelog](CHANGELOG.md)

## Authors

- [Dave Conroy](https://github.com/tiredofit)

## Table of Contents


- [Introduction](#introduction)
- [Authors](#authors)
- [Table of Contents](#table-of-contents)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
  - [Quick Start](#quick-start)
- [Configuration](#configuration)
  - [Data-Volumes](#data-volumes)
  - [Environment Variables](#environment-variables)
  - [Networking](#networking)
- [Maintenance](#maintenance)
  - [Shell Access](#shell-access)
- [References](#references)

## Prerequisites

This image assumes that you are using a reverse proxy such as
[jwilder/nginx-proxy](https://github.com/jwilder/nginx-proxy) and optionally the [Let's Encrypt Proxy
Companion @
https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion](https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion)
or [Traefik](https://github.com/tiredofit/docker-traefik) in order to serve your pages. However, it will run just fine on it's own if you map appropriate ports. See the examples folder for a docker-compose.yml that does not rely on a reverse proxy.

You will also need an external Postgresql container

## Installation

Automated builds of the image are available on [Docker Hub](https://hub.docker.com/r/tiredofit/baserow) and is the recommended method of installation.

```bash
docker pull tiredofit/baserow
```

### Quick Start

* The quickest way to get started is using [docker-compose](https://docs.docker.com/compose/). See the examples folder for a working [docker-compose.yml](examples/docker-compose.yml) that can be modified for development or production use.

* Set various [environment variables](#environment-variables) to understand the capabilities of this image.
* Map [persistent storage](#data-volumes) for access to configuration and data files for backup.
* Make [networking ports](#networking) available for public access if necessary

Once installed visit your "APPLICATION_HOSTNAME" environment variable with a web browser and proceed with registering a user account for yourself.

**The first boot can take from 2 minutes - 5 minutes depending on your CPU to setup the proper schemas.**



## Configuration

### Data-Volumes

The following directories are used for configuration and can be mapped for persistent storage.

| Directory   | Description               |
| ----------- | ------------------------- |
| `/www/logs` | Nginx Logfiles            |
| `/data`     | Volatile Files like Media |
### Environment Variables

Along with the Environment Variables from the [Base image](https://hub.docker.com/r/tiredofit/alpine) and [Web Image](https://hub.docker.com/r/tiredofit/nginx) below is the complete list of available options that can be used to customize your installation.


| Parameter                  | Description                                                                                 | Default       |
| -------------------------- | ------------------------------------------------------------------------------------------- | ------------- |
| `API_HOSTNAME`             | Api Hostname e.g. `api.example.com`                                                         |               |
| `API_PROTOCOL`             | API protocol `http` or `https`                                                              | `https`       |
| `APPLICATION_HOSTNAME`     | Application Hostname e.g. `baserow.example.com`                                             |               |
| `APPLICATION_PROTOCOL`     | Application protocol `http` or `https`                                                      | `https`       |
| `APP_DEBUG`                | Application Debug Mode - Do not enable on Production                                        | `False`       |
| `BACKEND_WORKERS`          | Backend API worker processes to spawn                                                       | `5`           |
| `DB_HOST`                  | Host or container name of Postgresql Server e.g. `baserow-db`                               |               |
| `DB_NAME`                  | Postgresql Database name e.g. `baserow`                                                     |               |
| `DB_PASS`                  | Postgresql Password for above Database e.g. `password`                                      |               |
| `DB_PORT`                  | Postgresql Server Port - Default `5432`                                                     | `5432`        |
| `DB_TYPE`                  | Database Type - Only `postgresql` supported at this time                                    | `postgresql`  |
| `DB_USER`                  | Postgresql Username for above Database e.g. `baserow`                                       |               |
| `INTERNAL_API_HOST`        | If seperating the container via `MODE` the hostname of the internal API server              | `localhost`   |
| `INTERNAL_API_LISTEN_PORT` | If seperating the container via `MODE` the listening port of the internal API server        | `8000`        |
| `INTERNAL_API_PROTOCOL`    | If seperating the container via `MODE` the protocol of the internal API server              | `http`        |
| `LANGUAGE`                 | Application Language                                                                        | `en-us`       |
| `LOG_LEVEL`                | Log Level `debug` only at this time                                                         | `debug`       |
| `MODE`                     | Type of Installation `AIO` (All in one), `FRONTEND` (Web Frontend), `BACKEND` (API Backend) | `AIO`         |
| `MEDIA_HOSTNAME`           | Media Hostname e.g. media.example.com                                                       |               |
| `MEDIA_LOCATION`           | Where Media files are saved on disk                                                         | `/data/media` |
| `PASSWORD_RESET_MAX_AGE`   | Password Reset Token Validity in hours                                                      | `1`           |

### Networking

The following ports are exposed.

| Port | Description |
| ---- | ----------- |
| `80` | HTTP        |


## Maintenance

### Shell Access

For debugging and maintenance purposes you may want access the containers shell.

```bash
docker exec -it (whatever your container name is e.g. baserow-app) bash
```

## References

* <https://baserow.io/>
