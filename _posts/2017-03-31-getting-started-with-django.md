---
bigimg: /img/bird-flying-start.jpg
image: /img/bird-flying-start.jpg
header_caption: 'soaring the skies'
comments: true
tags: [tech, Python, django]
---
In a quest of being adept at working with data, understanding a language & frameworks that works well
with numbers manipulation is essential.

<!--more-->


[Python][1] is a language vastly used within the data-science disciplines with lots of [libraries][2] and [packages][3] that support it.

[Django][4] is a web frontend framework that encourages rapid development with a clean pragmatic design

Before we get started, you'll need to have;

  - python (am using 3.6.0)
  - [pip][5]
  - [virtualenv][6]
  - Preferably linux based OS

Lets rumble..

#### Installations
Get stable versions of the above prerequisites.

Python :

python comes preinstalled in most os’s but you could always confirm your version using:
```
$ python

Python 3.6.0 (v3.6.0:41df79263a11, Dec 22 2016, 17:23:13)
[GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin
Type “help”, “copyright”, “credits” or “license” for more information.
>>>
```
if you see something like this, then python is installed. Exit this shell using :
```
ctrl + D  or  exit()
```
pip

pip is a package management system used to install and manage software packages written in Python.

virtualenv :

It is recommended that you use virtualenv to create isolated Python environments,

so you can use different package versions for different projects

install by Running the following command in your shell:

```$ pip install virtualenv```

After installing virtualenv, setting up our working environment would be the next task once through with the installations.

#### setup working environment
Create a directory where your project will reside in then work from within it with :

```
$ mkdir myproject


$ cd myproject
```

create an isolated environment with :

```$ virtualenv my_env```

activate the virtual environment by running :

```$ source my_env/bin/activate```

The shell will now include the name of the active virtual environment enclosed in parenthesis like:

```
(my_env):myproject $
```

you can always deactivate environments with ``deactivate`` command.

using pip , we installed earlier, get on installing Django with the following command,

```(my_env):myproject $ pip install Django```

To be certain, check that has been installed successfully by running:

```
(my_env):myproject $ python

>>> import django
>>> django.VERSION
(1, 10, 5, ‘final’, 0)
>>>
```

with an active environment, you can now embark on that project you are building.

#### start your project

begin by running :

```(my_env):myproject $ django-admin startproject mystic_falls```

this creates a django project by the name *mystic_falls* which has a basic structure

.(use whatever name you prefer for your project).

run the command below to work from project root folder and initialize application database :

```
(my_env)4bic:myproject $ cd mystic_falls

(my_env):mystic_falls $ python manage.py migrate
```
#### run server

The development server is ready to fire up, run:

```(my_env):mystic_falls $ python manage.py runserver```

on your browser, open : ```http://127.0.0.1:8000/ ```, you should see:

.. “Welcome to Django” page.

if you have gotten this far, you definitely are now ready to create you own app.

### Next Step : Creating an application

In Django, Applications provide some specific functionalities, think of the project as your website, which contains several applications like blog, wiki, or forum, which can be used in other projects.

We’ll create an app in subsequent blog


[1]: https://www.python.org
[2]: https://docs.python.org/3/library/
[3]: https://pypi.python.org/pypi
[4]: https://www.djangoproject.com
[5]: https://pip.pypa.io/en/stable/installing/
[6]: http://virtualenvwrapper.readthedocs.io/en/latest/
[7]: https://en.wikipedia.org/wiki/Package_manager
[8]: https://en.wikipedia.org/wiki/Package_%28package_management_system%29
[9]: https://en.wikipedia.org/wiki/Python_%28programming_language%29
