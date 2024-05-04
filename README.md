# Ghost + SQLite On PipeOps


Ghost is a free and open source blogging platform written in JavaScript and distributed under the MIT License, designed to simplify the process of online publishing for individual bloggers as well as online publications.


[![Deploy on PipeOps](https://pub-a1fbf367a4cd458487cfa3f29154ac93.r2.dev/Default.png)](#)


http://en.wikipedia.org/wiki/Ghost_%28blogging_platform%29



![](https://raw.githubusercontent.com/docker-library/docs/c5b6d94dc8f0557925ab37ca43141c0efc5cc363/ghost/logo.png)


# How to use this image

This will start a Ghost development instance listening on the default Ghost port of 2368.

   $ docker run -d --name some-ghost -e NODE_ENV=development ghost

# Custom port

If you'd like to be able to access the instance from the host without the container's IP, standard port mappings can be used:

   $ docker run -d --name some-ghost -e NODE_ENV=development -e url=http://localhost:3001 -p 3001:2368 ghost

If all goes well, you'll be able to access your new site on http://localhost:3001 and http://localhost:3001/ghost to access Ghost Admin (or http://host-ip:3001 and http://host-ip:3001/ghost, respectively).


## Upgrading Ghost

You will want to ensure you are running the latest minor version of Ghost before upgrading major versions. Otherwise, you may run into database errors.

For upgrading your Ghost container you will want to mount your data to the appropriate path in the predecessor container (see below): import your content from the admin panel, stop the container, and then re-mount your content to the successor container you are upgrading into; you can then export your content from the admin panel.


# Stateful

Mount your existing content. In this example we also use the Alpine Linux based image.

$ docker run -d \
	--name some-ghost \
	-e NODE_ENV=development \
	-e database__connection__filename='/var/lib/ghost/content/data/ghost.db' \
	-p 3001:2368 \
	-v /path/to/ghost/blog:/var/lib/ghost/content \
	ghost:alpine

Note: database__connection__filename is only valid in development mode and is the location for the SQLite database file. If using development mode, it should be set to a writeable path within a persistent folder (bind mount or volume). It is not available in production mode because an external MySQL server is required (see the docker-compose example below).


## Docker Volume

Alternatively you can use a named docker volume instead of a direct host path for /var/lib/ghost/content:

$ docker run -d \
	--name some-ghost \
	-e NODE_ENV=development \
	-e database__connection__filename='/var/lib/ghost/content/data/ghost.db' \
	-p 3001:2368 \
	-v some-ghost-data:/var/lib/ghost/content \
	ghost


# Configuration

All Ghost configuration parameters (such as url) can be specified via environment variables. See the Ghost documentation https://ghost.org/docs/concepts/config/#running-ghost-with-config-env-variables for details about what configuration is allowed and how to convert a nested configuration key into the appropriate environment variable name:

  $ docker run -d --name some-ghost -e NODE_ENV=development -e url=http://some-ghost.example.com ghost

(There are further configuration examples in the stack.yml listed below.)
