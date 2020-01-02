# Using LAVA dispatcher with Docker Compose

This repository attempts to provide a reference implementation of deploying
LAVA worker using it's [officially distributed docker container](https://docs.lavasoftware.org/lava/docker-admin.html#official-lava-software-docker-images).

## Requirements

Install the following.
- [Docker](https://docs.docker.com/install/)
- [docker-compose](https://docs.docker.com/compose/install/)

## Configuration

All configuration is stored in .env file. Some of the steps are required whilst
others are optional.

- Change LAVA_SERVER_HOSTNAME to <server_name> which points to the running
  LAVA master instance.
- (optional) set encryption to `--encrypt` if the master instance is using
  encryption for master-slave communication.
- (optional) [Create certificates](https://validation.linaro.org/static/docs/v2/pipeline-server.html#create-certificates) on the slave.
  `sudo /usr/share/lava-dispatcher/create_certificate.py foo_slave_1`
  This can be done in two ways:
  - by running "docker-compose run dispatcher bash".
  - alternatively you can create the certificates on system which has LAVA
    packages already installed.
- (optional) Copy public certificate from master and the private slave
  certificate created in previous step to directory `dispatcher/certs/` of this
  project. Currently the key names should be the default ones (master.key and
  slave.key_secret).
- Execute `make run`; at this point multiple containers should be up and
  running and the worker should connect to the LAVA server instance of you
  choosing.
- Add a new device and set its' device template (alternatively you can update
  existing device to use this new worker)
  Example QEMU device template:
  ```
  {% extends 'qemu.jinja2' %}
  {% set mac_addr = 'DF:AD:BE:EF:33:02' %}
  {% set memory = 1024 %}
  ```
- (optional) If the lab where this container runs in has a proxy or you require
  any specific worker environment settings, you will need to update the proxy
  settings by setting the [worker environment](https://validation.linaro.org/static/docs/v2/proxy.html#using-the-http-proxy)
  You can do this via this [XMLRPC API call](https://validation.linaro.org/api/help/#scheduler.workers.set_env).

## Usage

`make`: Services are built and then tagged. Following images are added to the
        system:
        - debian
        - httpd
        - lavasoftware/lava-dispatcher
        - lava-docker-compose-dispatcher_ser2net
        - lava-docker-compose-dispatcher_worker-tftpd
        - lava-docker-compose-dispatcher_worker-webserver
        - lava-docker-compose-dispatcher_dispatcher

`make run`: Run the following containers
 - ser2net
 - lava dispatcher
 - lava dispatcher http server
 - lava dispatcher tftp server

`make clean`: Permanently delete the containers and volumes.
