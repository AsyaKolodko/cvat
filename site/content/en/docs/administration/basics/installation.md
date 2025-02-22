<!--lint disable maximum-heading-length-->

---

title: 'Installation Guide'
linkTitle: 'Installation Guide'
weight: 1
description: 'A CVAT installation guide for different operating systems.'

---

<!--lint disable heading-style-->

# Quick installation guide

Before you can use CVAT, you’ll need to get it installed. The document below
contains instructions for the most popular operating systems. If your system is
not covered by the document it should be relatively straight forward to adapt
the instructions below for other systems.

Probably you need to modify the instructions below in case you are behind a proxy
server. Proxy is an advanced topic and it is not covered by the guide.

For access from China, read [sources for users from China](#sources-for-users-from-china) section.

## Ubuntu 18.04 (x86_64/amd64)

1. Open a [terminal window](https://askubuntu.com/questions/183775/how-do-i-open-a-terminal).
2. Install `docker`.

  ```shell
  sudo apt-get update
  sudo apt-get --no-install-recommends install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
  sudo add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) \
    stable"
  sudo apt-get update
  sudo apt-get --no-install-recommends install -y docker-ce docker-ce-cli containerd.io
  ```
  [More instructions](https://docs.docker.com/install/linux/docker-ce/ubuntu/).
3. Perform [post-installation steps](https://docs.docker.com/install/linux/linux-postinstall/) to run `docker` without root permissions.

  ```shell
  sudo groupadd docker
  sudo usermod -aG docker $USER
  ```

  Log out and log back in (or reboot) so that your group membership is
  re-evaluated. Type `groups` command in a terminal window after
  that and check if `docker` group is in its output.

4. Install `docker-compose` (1.19.0 or newer).
  ```shell
  sudo apt-get --no-install-recommends install -y python3-pip python3-setuptools
  sudo python3 -m pip install setuptools docker-compose
  ```

5. Clone _CVAT_ source code from the
  [GitHub repository](https://github.com/opencv/cvat) with Git.

6. Clone latest _develop_ branch:
  ```shell
  git clone https://github.com/opencv/cvat
  cd cvat
  ```

  See [alternatives](#how-to-get-cvat-source-code) if you want to download one of the release
  versions or use the `wget` or `curl` tools.

7. To access CVAT over a network or through a different system, export `CVAT_HOST` environment variable.

  ```shell
  export CVAT_HOST=your-ip-address
  ```

8. Run docker containers.

  ```shell
  docker-compose up -d
  ```

9. (Optional) Use the `CVAT_VERSION` environment variable to specify the version of CVAT you want to
  install (e.g `v2.1.0`, `dev`).
  By default the `dev` images are pulled for the _develop_ branch,
  and corresponding release images for the release versions.
  ```shell
  CVAT_VERSION=dev docker-compose up -d
  ```

>  [Build the images locally with unreleased changes](#how-to-pullbuildupdate-cvat-images)

10. Create a superuser. A superuser can use an admin panel to assign correct groups to a user.

  ```shell
  docker exec -it cvat_server bash -ic 'python3 ~/manage.py createsuperuser'
  ```

  Choose a username and a password for your admin account. For more information read [Django documentation](https://docs.djangoproject.com/en/2.2/ref/django-admin/#createsuperuser).

11. Install **Google Chrome** as it is the only browser which is supported by CVAT.

  ```shell
  curl https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
  sudo sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list'
  sudo apt-get update
  sudo apt-get --no-install-recommends install -y google-chrome-stable
  ```

12. Open Google Chrome browser and go to [localhost:8080](http://localhost:8080).
13. Log in as the superuser. Now you can create a new annotation task.

## Windows 10

1. [Install](https://docs.microsoft.com/windows/wsl/install-win10) WSL2 (Windows subsystem for Linux).
  > WSL2 requires Windows 10, version 2004 or higher.
  > You may not have to install a Linux distribution unless needed.

2. Download and install [Docker Desktop for Windows](https://download.docker.com/win/stable/Docker%20Desktop%20Installer.exe).
  
  * [More instructions](https://docs.docker.com/docker-for-windows/install/).
  * [Official guide for docker WSL2 backend](https://docs.docker.com/docker-for-windows/wsl/).
 
 > Check that you are specifically using WSL2 backend for Docker.

3. Download and install [Git for Windows](https://github.com/git-for-windows/git/releases/download/v2.21.0.windows.1/Git-2.21.0-64-bit.exe).
  When installing the package, keep all options by default.
  [More information about the package](https://gitforwindows.org).

4. Download and install [Google Chrome](https://www.google.com/chrome/). It is the only browser which is supported by CVAT.

5. Open the _Git Bash_ application.

6. Clone _CVAT_ source code from the [GitHub repository](https://github.com/opencv/cvat).

7. Clone the latest _develop_ branch:
  ```shell
  git clone https://github.com/opencv/cvat
  cd cvat
  ```

  See [alternatives](#how-to-get-cvat-source-code) if you want to download one of the release
  versions.

8. Run the `docker` containers.

  ```shell
  docker-compose up -d
  ```

9. (Optional) Use `CVAT_VERSION` environment variable to specify the version of CVAT you want to install (e.g `v2.1.0`, `dev`).
  By default `dev` images will be pulled for develop branch, and corresponding release images for release versions.

  ```shell
  CVAT_VERSION=dev docker-compose up -d
  ```

> [Building the images locally with unreleased changes](#how-to-pullbuildupdate-cvat-images)

10. Create a superuser. A superuser can use an admin panel to assign correct groups to a user.

  ```shell
  winpty docker exec -it cvat_server bash -ic 'python3 ~/manage.py createsuperuser'
  ```

  If you don't have `winpty` installed or the above command does not work, you may also try the following:

  ```shell
  # enter docker image first
  docker exec -it cvat_server /bin/bash
  # then run
  python3 ~/manage.py createsuperuser
  ```

  Choose a username and a password for your admin account. For more information read [Django documentation](https://docs.djangoproject.com/en/2.2/ref/django-admin/#createsuperuser).

11. Open Google Chrome browser and go to [localhost:8080](http://localhost:8080).
12. Log in as the superuser. Now you can create a new annotation task.

## Mac OS Mojave

1. Download [Docker for Mac](https://download.docker.com/mac/stable/Docker.dmg).
  [More instructions](https://docs.docker.com/v17.12/docker-for-mac/install/#install-and-run-docker-for-mac).

2. Install the _Xcode Command Line Tools_. On Mavericks (10.9) or above run `git` from **Terminal**.

  ```shell
  git --version
  ```

  If you don’t have it installed already, it will prompt you to install it.
  [More instructions](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

3. Download and install [Google Chrome](https://www.google.com/chrome/), as it is the only browser supported by CVAT.

4. Open **Terminal**.

5. Clone _CVAT_ source code from the [GitHub repository](https://github.com/opencv/cvat) with Git.

  Clone the latest _develop_ branch:
  ```shell
  git clone https://github.com/opencv/cvat
  cd cvat
  ```
  See [alternatives](#how-to-get-cvat-source-code) if you want to download one of the release
  versions or use the `wget` or `curl` tools.

6. Run the `docker` containers.

  ```shell
  docker-compose up -d
  ```

7. (Optional) Use `CVAT_VERSION` environment variable to specify the version of CVAT you want to install (e.g `v2.1.0`, `dev`).
  By default the `dev` images are pulled for develop branch, and corresponding release images for release versions.

  ```shell
  CVAT_VERSION=dev docker-compose up -d
  ```

> [Building the images locally with unreleased changes](#how-to-pullbuildupdate-cvat-images)

8. Create a superuser. A superuser can use an admin panel to assign correct groups to a user.

  ```shell
  docker exec -it cvat_server bash -ic 'python3 ~/manage.py createsuperuser'
  ```

  Choose a username and a password for your admin account. For more information read [Django documentation](https://docs.djangoproject.com/en/2.2/ref/django-admin/#createsuperuser).

9. Open Google Chrome browser and go to [localhost:8080](http://localhost:8080).
10. Log in as the superuser. Now you can create a new annotation task.

## Advanced Topics

### How to get CVAT source code

#### Git (Linux, Mac, Windows)

1. Install Git on your system if it's not already installed
   - Ubuntu:
   ```shell
   sudo apt-get --no-install-recommends install -y git
   ```

   - Windows:
   Follow instructions from [https://git-scm.com/download/win](https://git-scm.com/download/win)

2. Clone _CVAT_ source code from the
   [GitHub repository](https://github.com/opencv/cvat).

   The command below will clone the default branch (develop):
   ```shell
   git clone https://github.com/opencv/cvat
   cd cvat
   ```

   To clone specific tag, e.g. v2.1.0:
   ```shell
   git clone -b v2.1.0 https://github.com/opencv/cvat
   cd cvat
   ```

#### Wget (Linux, Mac)

To download latest develop branch:
```shell
wget https://github.com/opencv/cvat/archive/refs/heads/develop.zip
unzip develop.zip && mv cvat-develop cvat
cd cvat
```

To download specific tag:
```shell
wget https://github.com/opencv/cvat/archive/refs/tags/v1.7.0.zip
unzip v1.7.0.zip && mv cvat-1.7.0 cvat
cd cvat
```

#### Curl (Linux, Mac)

To download latest develop branch:
```shell
curl -LO https://github.com/opencv/cvat/archive/refs/heads/develop.zip
unzip develop.zip && mv cvat-develop cvat
cd cvat
```

To download specific tag:
```shell
curl -LO https://github.com/opencv/cvat/archive/refs/tags/v1.7.0.zip
unzip v1.7.0.zip && mv cvat-1.7.0 cvat
cd cvat
```

### Deploying CVAT behind a proxy

If you deploy CVAT behind a proxy and do not plan to use any of [serverless functions](#semi-automatic-and-automatic-annotation)
for automatic annotation, the exported environment variables
`http_proxy`, `https_proxy` and `no_proxy` should be enough to build images.
Otherwise please create or edit the file `~/.docker/config.json` in the home directory of the user
which starts containers and add JSON such as the following:

```json
{
  "proxies": {
    "default": {
      "httpProxy": "http://proxy_server:port",
      "httpsProxy": "http://proxy_server:port",
      "noProxy": "*.test.example.com,.example2.com"
    }
  }
}
```

These environment variables are set automatically within any container.
Please see the [Docker documentation](https://docs.docker.com/network/proxy/) for more details.

### Using the Traefik dashboard

If you are customizing the docker compose files and you come upon some unexpected issues, using the Traefik
dashboard might be very useful to see if the problem is with Traefik configuration, or with some of the services.

You can enable the Traefik dashboard by uncommenting the following lines from `docker-compose.yml`

```yml
services:
  traefik:
    # Uncomment to get Traefik dashboard
    #   - "--entryPoints.dashboard.address=:8090"
    #   - "--api.dashboard=true"
    # labels:
    #   - traefik.enable=true
    #   - traefik.http.routers.dashboard.entrypoints=dashboard
    #   - traefik.http.routers.dashboard.service=api@internal
    #   - traefik.http.routers.dashboard.rule=Host(`${CVAT_HOST:-localhost}`)
```

and if you are using `docker-compose.https.yml`, also uncomment these lines
```yml
services:
  traefik:
    command:
      # Uncomment to get Traefik dashboard
      # - "--entryPoints.dashboard.address=:8090"
      # - "--api.dashboard=true"
```

Note that this "insecure" dashboard is not recommended in production (and if your instance is publicly available);
if you want to keep the dashboard in production you should read Traefik's
[documentation](https://doc.traefik.io/traefik/operations/dashboard/) on how to properly secure it.

### Additional components

- [Analytics: management and monitoring of data annotation team](/docs/administration/advanced/analytics/)

```shell
# Build and run containers with Analytics component support:
docker-compose -f docker-compose.yml \
  -f components/analytics/docker-compose.analytics.yml up -d --build
```

### Semi-automatic and automatic annotation

Please follow this [guide](/docs/administration/advanced/installation_automatic_annotation/).

### Stop all containers

The command below stops and removes containers and networks created by `up`.

```shell
docker-compose down
```

### Use your own domain

If you want to access your instance of CVAT outside of your localhost (on another domain),
you should specify the `CVAT_HOST` environment variable, like this:

```shell
export CVAT_HOST=<YOUR_DOMAIN>
```

### Share path

You can use a share storage for data uploading during you are creating a task.
To do that you can mount it to CVAT docker container. Example of
docker-compose.override.yml for this purpose:

```yml
version: '3.3'

services:
  cvat_server:
    volumes:
      - cvat_share:/home/django/share:ro
  cvat_worker_default:
    volumes:
      - cvat_share:/home/django/share:ro

volumes:
  cvat_share:
    driver_opts:
      type: none
      device: /mnt/share
      o: bind
```

You can change the share device path to your actual share.

You can [mount](/docs/administration/advanced/mounting_cloud_storages/)
your cloud storage as a FUSE and use it later as a share.

### Email verification

You can enable email verification for newly registered users.
Specify these options in the
[settings file](https://github.com/cvat-ai/cvat/blob/develop/cvat/settings/base.py) to configure Django allauth
to enable email verification (ACCOUNT_EMAIL_VERIFICATION = 'mandatory').
Access is denied until the user's email address is verified.

```python
ACCOUNT_AUTHENTICATION_METHOD = 'username'
ACCOUNT_CONFIRM_EMAIL_ON_GET = True
ACCOUNT_EMAIL_REQUIRED = True
ACCOUNT_EMAIL_VERIFICATION = 'mandatory'

# Email backend settings for Django
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
```

Also you need to configure the Django email backend to send emails.
This depends on the email server you are using and is not covered in this tutorial, please see
[Django SMTP backend configuration](https://docs.djangoproject.com/en/3.1/topics/email/#django.core.mail.backends.smtp.EmailBackend)
for details.

### Deploy CVAT on the Scaleway public cloud

Please follow
[this tutorial](https://blog.scaleway.com/smart-data-annotation-for-your-computer-vision-projects-cvat-on-scaleway/)
to install and set up remote access to CVAT on a Scaleway cloud instance with data in a mounted object storage bucket.

### Deploy secure CVAT instance with HTTPS

Using Traefik, you can automatically obtain TLS certificate for your domain from Let's Encrypt,
enabling you to use HTTPS protocol to access your website.

To enable this, first set the the `CVAT_HOST` (the domain of your website) and `ACME_EMAIL`
(contact email for Let's Encrypt) environment variables:

```shell
export CVAT_HOST=<YOUR_DOMAIN>
export ACME_EMAIL=<YOUR_EMAIL>
```

Then, use the `docker-compose.https.yml` file to override the base `docker-compose.yml` file:

```shell
docker-compose -f docker-compose.yml -f docker-compose.https.yml up -d
```

> In firewall, ports 80 and 443 must be open for inbound connections from any

Then, the CVAT instance will be available at your domain on ports 443 (HTTPS) and 80 (HTTP, redirects to 443).

### How to pull/build/update CVAT images

- **For a CVAT version lower or equal to 2.1.0**, you need to pull images using docker because
  the compose configuration always points to the latest image tag, e.g.
  ```shell
  docker pull cvat/server:v1.7.0
  docker tag cvat/server:v1.7.0 openvino/cvat_server:latest

  docker pull cvat/ui:v1.7.0
  docker tag cvat/ui:v1.7.0 openvino/cvat_ui:latest
  ```
  **For CVAT version more than v2.1.0** it's possible to pull specific version of
  prebuilt images from DockerHub using `CVAT_VERSION` environment variable to specify
  the version (e.g. `dev`):
  ```shell
  CVAT_VERSION=dev docker-compose pull
  ```

- To build images yourself include `docker-compose.dev.yml` compose config file to `docker-compose` command.
  This can be useful if you want to build a CVAT with some source code changes.
  ```shell
  docker-compose -f docker-compose.yml -f docker-compose.dev.yml build
  ```
- To update local images to `latest` or `dev` tags run:
  ```shell
  CVAT_VERSION=dev docker-compose pull
  ```
  or
  ```shell
  CVAT_VERSION=latest docker-compose pull
  ```

## Troubleshooting

### Sources for users from China

If you stay in China, for installation you need to override the following sources.

- For use `apt update` using:

  [Ubuntu mirroring help](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)

  Pre-compiled packages:
  ```shell
  deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
  deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
  deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
  deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse
  ```
  Or source packages:
  ```shell
  deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
  deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
  deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
  deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse
  ```

- [Docker mirror station](https://www.daocloud.io/mirror)

  Add registry mirrors into `daemon.json` file:
  ```json
  {
      "registry-mirrors": [
          "http://f1361db2.m.daocloud.io",
          "https://docker.mirrors.ustc.edu.cn",
          "https://hub-mirror.c.163.com",
          "https://https://mirror.ccs.tencentyun.com",
          "https://mirror.ccs.tencentyun.com",
      ]
  }
  ```

- For using `pip`:

  [PyPI mirroring help](https://mirrors.tuna.tsinghua.edu.cn/help/pypi/)
  ```shell
  pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
  ```

- For using `npm`:

  [npm mirroring help](https://npmmirror.com/)
  ```shell
  npm config set registry https://registry.npm.taobao.org/
  ```

- Instead of `git` using [`gitee`](https://gitee.com/):

  [CVAT repository on gitee.com](https://gitee.com/monkeycc/cvat)

- For replace acceleration source `docker.com` run:
  ```shell
  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
  sudo add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) \
  ```

- For replace acceleration source `google.com` run:
  ```shell
  curl https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
  ```

### HTTPS is not working because of a certificate

If you're having trouble with SSL connection, to find the cause,
you'll need to get the logs from traefik by running:

```shell
docker logs traefik
```

The logs will help you find out the problem.

If the error is related to a firewall, then:
- Open ports 80 and 443 for inbound connections from any.
- Delete `acme.json`.
  The location should be something like: `/var/lib/docker/volumes/cvat_cvat_letsencrypt/_data/acme.json`.

After `acme.json` is removed, stop all cvat docker containers:

```shell
docker-compose -f docker-compose.yml -f docker-compose.https.yml down
```

Make sure variables set (with your values):

```shell
export CVAT_HOST=<YOUR_DOMAIN>
export ACME_EMAIL=<YOUR_EMAIL>
```

and restart docker:

```shell
docker-compose -f docker-compose.yml -f docker-compose.https.yml up -d
```
