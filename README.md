OpenFaaS Waitress (WSGI) template
=============================================

This is a function template for the [OpenFaas platform](https://www.openfaas.com/). It is heavily based on the python-flask templates that can be found [here](https://github.com/openfaas-incubator/python-flask-template).

This template allows you to easily serve a [Django project](https://www.djangoproject.com/) via the WSGI interface using [Waitress](https://github.com/Pylons/waitress). Other WSGI compatible web frameworks should be possible as well, but are not explained in this README file.

Note that this template makes use of the incubator project [of-watchdog](https://github.com/openfaas-incubator/of-watchdog).

Templates available in this repository:
- python-waitress

## Getting started

Note that an OpenFaas function name `<function-name>` cannot have underscores, while a Django project name `<django-name>` cannot have dashes.

Download the template
```
$ faas template pull https://github.com/aabversteeg/openfaas-waitress
```

Create a new function
```
$ faas new --lang python-waitress <function-name>
```

Create the Django project
```
$ django-admin startproject <django-name> <function-name>
```

Adjust Django's settings in <function-name>/<django-name>/settings.py
```
ALLOWED_HOSTS = ["<function-name>"]
FORCE_SCRIPT_NAME = "/function/<function-name>/"
```

Add the WSGI_APP environment variable to the function's yaml file (i.e. <function-name>.yml)
```
version: 1.0
provider:
  name: openfaas
  gateway: http://127.0.0.1:8080
functions:
  <function-name>:
    lang: python-waitress
    handler: ./<function-name>
    image: <function-name>:latest
    environment:
      WSGI_APP: <django-name>.wsgi:application
``` 

Build, and deploy
```
$ faas build -f <function-name>.yml
$ faas deploy -f <function-name>.yml
```

Test the new function by navigating to: http://127.0.0.1:8080/function/<function-name>
