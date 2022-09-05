# Docker for local development for ProcessMaker 3 (Stack-N275 alternative)

This repository is for development purposes

## Content

This branch contains a [three-tier architecture] application running on a LEMP stack, supported by Docker and orchestrated by Docker Compose.

It includes:

* A container for Nginx;
* A container for the backend application (based on [Laravel](https://laravel.com/));
* A container for MySQL;
* A container for phpMyAdmin;
* A volume to persist MySQL data.

The containers are based on Alpine images when available, for an optimised size.

## Prerequisites

Make sure [Docker Desktop for Mac or PC](https://www.docker.com/products/docker-desktop) is installed and running, or head [over here](https://docs.docker.com/install/) if you are a Linux user. You will also need a terminal running [Git](https://git-scm.com/).

This setup also uses localhost's port 3000, so make sure it is available.

## Directions of use

Add the following domains to your machine's `hosts` file:

```
127.0.0.1 processmaker.demo.test phpmyadmin.demo.test
```

Clone the repository and `checkout` the `main` branch:

```
$ git clone https://github.com/rodriquelca/docker4me.git && cd docker4me
$ git checkout main
```

Copy .env.example to .env:

```
$ cp .env.example .env

```
To ensure compatibility across operating systems, specify your system's current user ID in the newly created `.env` at the root of the project, e.g.:

```
HOST_UID=501
```

You can obtain that ID by running the following command in a terminal:

```
$ id -u
```

Run the following command:
```
$ docker compose up -d
```

**The rest of the commands are to be run from the root of the project.**

Create the processmaker projects
```
$ docker exec -it demo-processmaker-1 bash
$ mkdir src
$ cd src
```
Create a ssh key [git key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

Add this key your bitbucket settings, then clone the processmaker project
```
$ git clone git@bitbucket.org:colosa/processmaker.git
$ exit
```


The following commands may take a little bit of time, as some Docker images might need downloading.

Install the backend dependencies:

```
$ docker compose run --rm processmaker composer install
```

Install the frontend dependencies:

```
$ docker compose run --rm processmaker rake
```

Start the project:

``` 
$ docker compose up -d
```




## Cleaning up

To stop the containers:

```
$ docker compose stop
```

To destroy the containers:

```
$ docker compose down
```

To destroy the containers and the associated volumes:

```
$ docker compose down -v
```

To remove everything, including images and orphan containers:

```
$ docker compose down -v --rmi all --remove-orphans
```
