# Treads

App for tire tread depth monitoring

## Getting started

1. Clone this repository.
2. Activate Python environment.

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
