# remote-docker

A Docker container to securely control a remote docker daemon CLI using ssh forwarding, no SSL setup needed.

## Usage

Lets assume you want to control the docker daemon on your `webserver.com` server. You need to run a `codefresh/remote-docker` Docker container, passing `ssh` private key file to it, like this:

    $ docker run -it --rm -v ${HOME}/.ssh/id_rdocker:/root/.ssh/id_rdocker codefresh/remote-docker rdocker user@webserver.com

Or you can pass `ssh` key as the environment variable (assuming you've setup `SSH_KEY`)

    $ docker run -it --rm -e SSH-KEY=${SSH_KEY} codefresh/remote-docker rdocker user@webserver.com

This will open a new `bash` session, within the `codefresh/remote-docker` container, with a new `DOCKER_HOST` variable setup. Any `docker` command you execute will take place on the remote docker daemon.
To test the connection run:

    docker:user@webserver.com bash-4.3# docker info

Check the `Name:` field it should have the remote hostname .... That's it!!!

You could for example run `docker build` to build an image on the remote host and then `docker save -o myimage.tar image_name` to store it locally.
Or run `docker exec -it container_name bash` to open a shell session on a remote container. Even bash auto-completion works ok.

To stop controlling the remote daemon and close the ssh forwarding, just exit the newly created bash session (press `Ctrl+D`).

You can also execute any `docker` command directly, without openning a `bash` shell in the `codefresh/remote-docker` container.

    $ docker run -it --rm -v ${HOME}/.ssh/id_rdocker:/root/.ssh/id_rdocker codefresh/remote-docker rdocker user@webserver.com docker info

## Used software

- Copy of `rdocker.sh` script (MIT license) from (dvddarias/rdocker)[https://github.com/dvddarias/rdocker]
