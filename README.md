# Installing Mailcow and Mailman3 based on dockerized versions =

## Introduction

This guide aims to install and configure [mailcow-dockerized](https://github.com/mailcow/mailcow-dockerized) with [docker-mailman](https://github.com/maxking/docker-mailman) and to provide some useful scripts.

There are some guides and projects on the internet, but there are not up to date and/or incomplete in documentation or configuration. This guide is based on the work of:

- [mailcow-mailman3-dockerized](https://github.com/Shadowghost/mailcow-mailman3-dockerized) by [Shadowghost](https://github.com/Shadowghost)
- [mailman-mailcow-integration](https://gitbucket.pgollor.de/docker/mailman-mailcow-integration)

After finishing this guide, [mailcow-dockerized](https://github.com/mailcow/mailcow-dockerized) and [docker-mailman](https://github.com/maxking/docker-mailman) will run and *Apache* as a reverse proxy will serve the web frontends.

## Installation

This guide ist based on different steps:

1. DNS setup
1. Install *Apache* as a reverse proxy
1. Obtain ssl certificates with *Let's Encrypt*
1. Install *Mailcow* with *Mailman* integration
1. Install *Mailman*
1. Run

### DNS setup

Most of the configuration ist covered by *Mailcow*s [DNS setup](https://mailcow.github.io/mailcow-dockerized-docs/prerequisite-dns/). After finishing this setup add another subdomain for *Mailcow*, e.g. `lists.example.org` that points to the same server:

```
# Name    Type       Value
lists     IN A       1.2.3.4
lists     IN AAAA    dead:beef
```

### Install *Apache* as a reverse proxy

Install *Apache*, e.g. with this guide from *Digital Ocean*: [How To Install the Apache Web Server on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-ubuntu-20-04).

Activate certain *Apache* modules (as *root* or *sudo*):

```
a2enmod rewrite proxy proxy_http headers ssl wsgi proxy_uwsgi http2
```

Maybe you have to install further packages to get these modules. This [PPA](https://launchpad.net/~ondrej/+archive/ubuntu/apache2) by *Ondřej Surý* may help you.

#### vhost configuration

Copy the `apache/mailcow.conf` and the `apache/mailman.conf` to the *Apache* conf folder `sites-available` (e.g. under `/etc/apache2/sites-available`).

Change in `mailcow.conf`:
- `MAILCOW_HOSTNAME` to your **MAILCOW_HOSTNAME**

Change in `mailman.conf`:
- `MAILMAN_DOMAIN` to your *Mailman* domain (e.g. `lists.example.org`)

**Don't activate the configuration, as the ssl certificates and directories are missing yet.**


### Obtain ssl certificates with *Let's Encrypt*

**go on here**


### Install *Mailcow* with *Mailman* integration

### Install *Mailman*


### Run

Run (as *root* or *sudo*)

```
a2ensite mailcow.conf
a2ensite mailman.conf
systemctl restart apache2

cd /opt/mailcow-dockerized/
docker-compose pull
docker-compose up -d

cd /opt/docker-mailman
docker-compose pull
docker-compose up -d
```

## Update

## Backup

Mailman:
https://gitbucket.pgollor.de/docker/mailman-mailcow-integration/blob/master/mailman-backup.sh
