# Docker for local pm4 development, smoothing things out with Bash

## Content

This branch contains a [three-tier architecture](https://www.techopedia.com/definition/24649/three-tier-architecture) application running on a LEMP stack, supported by Docker and orchestrated by Docker Compose.

It includes:

* A container for Nginx;
* A container for the backend application (based on [Laravel](https://laravel.com/));
* A container for MySQL;
* A container for phpMyAdmin;
* A volume to persist MySQL data.

The containers are based on Alpine images when available, for an optimised size.

## Prerequisites

Make sure [Docker Desktop for Mac or PC](https://www.docker.com/products/docker-desktop) is installed and running, or head [over here](https://docs.docker.com/install/) if you are a Linux user. You will also need a terminal running [Git](https://git-scm.com/) and [Bash](https://www.gnu.org/software/bash/).

This setup also uses localhost's port 80, so make sure it is available.

## Directions of use

Add the following domains to your machine's `hosts` file:

```
127.0.0.1 processmaker.demo.test  phpmyadmin.demo.test
```

Clone the repository and `checkout` the `main` branch:

```
$ git clone https://github.com/rodriquelca/docker4me.git && cd docker4me
$ git checkout main
$ cd pm4_php8.1
```

Add the following function to your Bash start-up file (`.bashrc`, `.zshrc`...):

```bash
function demo {
    cd <PATH>/pm4_php8.1 && bash demo $*
        cd -
}
```

Where `<PATH>` is the absolute path leading to the folder where the repository was cloned.

To ensure compatibility across operating systems, you will also need to export the system's current user ID. Add this line to the same file:

```bash
export HOST_UID=$(id -u)
```

Open a new terminal window or `source` your Bash start-up file for the changes to take effect, then run the following command:

```
$ demo init
```

This will download and build the images listed in `docker-compose.yml`, create and start the corresponding containers, and take other various steps to set up the project (this might take a while).

Once the script is done, you can visit [processmaker.demo.test](http://processmaker.demo.test).

Learn about the available commands by displaying the menu:

```
$ demo
```

## Explanation

The images used by the setup are listed and configured in [`docker-compose.yml`](https://github.com/rodriquelca/docker4me/blob/main/pm4_php8.1/docker-compose.yml).

When building and starting the containers based on the images for the first time, a MySQL database named `demo` is automatically created (you can pick a different name in the MySQL service's description in `docker-compose.yml`).

Minimalist Nginx configurations for the [processmaker application](https://github.com/rodriquelca/docker4me/blob/main/pm4_php8.1/docker-compose.yml) and [phpMyAdmin](https://github.com/rodriquelca/docker4me/blob/main/pm4_php8.1/.docker/nginx/conf.d/phpmyadmin.conf) are also copied over to Nginx's container, making them available at [processmaker.demo.test](http://processmaker.demo.test),  [phpmyadmin.demo.test](http://phpmyadmin.demo.test) respectively (the database credentials are *root* / *root*).

The directories containing the backend and frontend applications are mounted onto both Nginx's and the applications' containers, meaning any update to the code is immediately available upon refreshing the page, without having to rebuild any container.

Moreover, the Vue.js development server is automatically started, meaning you can update the code and benefit from _hot-reload_ straight away.

The frontend application is consuming a simple endpoint from the backend application to fetch and display the text below the animated gif.

The database data is persisted in its own local directory through the volume `mysqldata`, which is mounted onto MySQL's container.



## Cleaning up

To stop the containers:

```
$ demo stop
```

To destroy the containers:

```
$ demo down
```

To destroy the containers and the associated volumes:

```
$ demo down -v
```

To remove everything, including images and orphans containers:

```
$ demo destroy
```
