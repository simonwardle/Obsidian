https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/#create-and-use-virtual-environments

To create a virtual environment, go to your project’s directory and run the following command. This will create a new virtual environment in a local folder named `.venv`:

```
python3 -m venv .venv
```

### Activate a virtual environment

Before you can start installing or using packages in your virtual environment you’ll need to `activate` it. Activating a virtual environment will put the virtual environment-specific `python` and `pip` executables into your shell’s `PATH`.


```
source .venv/bin/activate
```

To confirm the virtual environment is activated, check the location of your Python interpreter:

```
which python
```

While the virtual environment is active, the above command will output a filepath that includes the `.venv` directory, by ending with the following:

.venv/bin/python


While a virtual environment is activated, pip will install packages into that specific environment. This enables you to import and use packages in your Python application.

### Deactivate a virtual environment

If you want to switch projects or leave your virtual environment, `deactivate` the environment:

deactivate

#### Note

Closing your shell will deactivate the virtual environment. If you open a new shell window and want to use the virtual environment, reactivate it.

### Reactivate a virtual environment

If you want to reactivate an existing virtual environment, follow the same instructions about activating a virtual environment. There’s no need to create a new virtual environment.


## Prepare pip

[pip](https://packaging.python.org/en/latest/key_projects/#pip) is the reference Python package manager. It’s used to install and update packages into a virtual environment.

Unix/macOS

The Python installers for macOS include pip. On Linux, you may have to install an additional package such as `python3-pip`. You can make sure that pip is up-to-date by running:

```
python3 -m pip install --upgrade pip
python3 -m pip --version
```

Afterwards, you should have the latest version of pip installed in your user site:

pip 23.3.1 from .../.venv/lib/python3.9/site-packages (python 3.9)



## Install packages using pip

When your virtual environment is activated, you can install packages. Use the `pip install` command to install packages.

### Install a package

For example, let’s install the [Requests](https://pypi.org/project/requests/) library from the [Python Package Index (PyPI)](https://packaging.python.org/en/latest/glossary/#term-Python-Package-Index-PyPI):

```
python3 -m pip install requests
```

pip should download requests and all of its dependencies and install them:


## ==Using a requirements file

Instead of installing packages individually, pip allows you to declare all dependencies in a [Requirements File](https://pip.pypa.io/en/latest/user_guide/#requirements-files "(in pip v25.2)"). For example you could create a `requirements.txt` file containing:

requests2.18.4
google-auth1.1.0

And tell pip to install all of the packages in this file using the `-r` flag:

```
python3 -m pip install -r requirements.txt
```


## ==Create a requirements file

Requirements files are used to hold the result from [pip freeze](https://pip.pypa.io/en/latest/cli/pip_freeze/#pip-freeze) for the purpose of achieving [Repeatable Installs](https://pip.pypa.io/en/latest/topics/repeatable-installs/). In this case, your requirement file contains a pinned version of everything that was installed when `pip freeze` was run.



python -m pip freeze > requirements.txt