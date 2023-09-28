# Treads

App for tire tread depth monitoring

## Getting started

1. Clone this repository.
2. Activate Python environment.
3. Install Docker Desktop.
4. Run local database instance.

## Python environment

To avoid conflicts between different versions and dependencies of Python packages, it's a best practice to set a 
separate Python virtual environment for each project. Each project might also require a different version of Python. To
solve these two issues at hand, we're going to use `pyenv` and `pyenv-virtualenv`.

See [pyenv](https://github.com/pyenv/pyenv) and [pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv) for 
details, installation and activation. For now make sure to install both - just follow instructions and let me know if
you need any help or have any questions.

We're going to use the latest Python `3.11.4` as we're a modern team, using cutting-edge technologies 😬. To have this
information about Python version available for scripts, we're going to save it in `.python-version-real` file.

```shell
# Install Python version that we need.
pyenv install $(cat .python-version-real)
# Activate installed Python for current shell session.
pyenv shell $(cat .python-version-real)
# Check Python version.
python --version                                                                               
```

Confirm desired Python version.

> _NOTE: Command `cat` prints out the contents of given file. So `cat .python-real-version` will print `3.11.4`.
> Wrapping the command in $() will execute the command(s) in the brackets and will be replaced by the result. So
> `pyenv install $(cat .python-version-real)` is equivalent  of `pyenv install 3.11.4`._

Now we have Python of version this project needs installed on the system. But there might be multiple projects that 
could use this Python version, install different packages, and we would still get in trouble with the conflicts of 
dependencies. To solve this, we'll create a separate space for Python packages, only for this project - a Python 
virtual environment. The virtual environment will be based off the Python version we just installed. There are 
several implementations of Python virtual environments, but since we're already using `pyenv`, we're going to use their
virtualenv as well, and it happens to be one of the best implementations too.

```shell
# Create virtualenv `treads` (in `.python-version`) based on version `3.11.4` (in `.python-version-real`).
pyenv virtualenv $(cat .python-version-real) $(cat .python-version)
```

If you installed and activated `pyenv-virtualenv` correctly, it will automatically read the name of the virtualenv upon
entering a directory (`cd`) from `.python-version` file and activate it. Your prompt might look something like:

```shell
(treads) ➜  treads git:(main) $
```

which would indicate, that Python virtualenv called `treads` is active. You can also verify the activated environment
by checking which `python` binary is being used:

```shell
(treads) ➜  treads git:(feature/2-pyenv-setup) $ pyenv which python      
/home/david/.pyenv/versions/treads/bin/python
```

Calling `pyenv which python` will print path to Python binary, and we can see that it's located in "version" `treads` - 
i.e. in our virtualenv.


## Docker setup

Make sure you have [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed. You can verify it by
running following commands:

```shell
$ docker version                       
Client: Docker Engine - Community
 Version:           24.0.5
...

Server: Docker Engine - Community
 Engine:
  Version:          24.0.5
...

# Run hello-world:
$ docker run --rm hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
719385e32844: Pull complete 
Digest: sha256:4f53e2564790c8e7856ec08e384732aa38dc43c52f02952483e3f003afbf23db
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
 
# Docker compose:
$ docker compose version  
Docker Compose version v2.20.2
```

Instead of running containers manually with `docker run`, we will use more convenient and declarative way, using `docker
compose`. All parameters we would normally pass to `docker run` are contained in `docker-compose.yaml` file. But first
need to set up initial administrator password.

```shell
echo "change-me-this-is-secret-password" > .secrets/postgres-password.txt 

# Run all containers (and other Docker objects) defined in docker-compose.yaml in the background. 
$ docker compose up -d

# Show and follow logs from running containers.
$ docker compose logs -f

# List containers defined by docker-compose.yaml.
$ docker compose ps -a

# To terminate running containers (data volumes will be retained).
$ docker compose down
```

While its container is running, database will be accessible on `localhost:5432`, e.g. via [pgAdmin](https://www.pgadmin.org/),
using the password we set up in the `postgres-password.txt` file. 
