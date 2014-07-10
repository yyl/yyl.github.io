---
layout: post
title:  "Bootstrap Django web app on Apache"
date:   2014-07-02 11:20:00
categories: Server
---

The problem is to configure a Linux machine to server a Django web app, which talks to MySQL, using Apache. Also the Django app is in a Python virtual environment. I just want to take a note on how to that from scratch since I have been asked to do it several times.

### Prepare the machine

A couple of essential packages:

    sudo apt-get update
    sudo apt-get dist-upgrade
    sudo apt-get install mysql-server mysql-client
    sudo apt-get install apache2
    sudo apt-get install build-essential
    sudo apt-get install curl

Of course we need a root account. We also need mysql and apache, and `build-essential` includes a set of compilers such as `gcc`. Finally, `curl` is good for some basic http testing.

### Configure the server

Apache is supposed to be starting automatically after installation, so nothing to worry about in general. However I do confronted a problem once. After installation, apache is not running, and I cannot get it started by command `service start apache2`. When I checked the log `/var/logs/apache2/error.log` it says

    [warn] pid file /var/run/apache2.pid overwritten -- Unclean shutdown of previous Apache run?
    [alert] (11)Resource temporarily unavailable: apr_thread_create: unable to create worker thread
    [alert] (11)Resource temporarily unavailable: apr_thread_create: unable to create worker thread
    [notice] Apache/2.2.16 (Debian) configured -- resuming normal operations
    [alert] No active workers found... Apache is exiting!

As I googled it is due to limited memory allocation for running thread. The default setting for apache is `unlimited`, while in fact 512k should be sufficient for it to run normally. To set the limit, add `ulimit -s 512` to the file `/etc/apache2/envvars`. Then if you restart apache, you should see it running now. Also, you should be able to see "It works" web page when you access your server in your web browser. The web page is a default page locating at `/var/www/`. If you need to only host static site, you could just modify and add files to that folder.

### Python virtual environment

Now since we run python web app, we need to setup Python environment. In addition, I like to have my projects managed under virtual environment. A virtual environment is like an isolated working directory for one project, so that you could have better control over its dependency.

Let's install some packages first:

    sudo apt-get install python
    sudo apt-get install python-dev
    sudo apt-get install python-pip
    sudo pip install virtualenv
    sudo pip install virtualenvwrapper

We need Python of course. `python-dev` is a set of libraries and header files needed when compiling other packages. `pip` is a package manager for Python, as `aptitude` for Debian. In our virtual environment, we will use it to install and uninstall packages. The last two packages are for virtual environment. With them we could create, manage and remove virtual environment using command line tools.

Now to create a new virtual environment:

    mkdir Envs
    export WORKON_HOME=~/Envs
    source /usr/local/bin/virtualenvwrapper.sh
    mkvirtualenv Project1

`Envs` is gonna be the directory where all your projects locate at. The second line tells the virtual environment manager where to mange those environments. The third line activates commands to use. Finally, the last line creates a new virtual environment for you.

If you see the command line becomes something like `(Project1)root@server:~$`, it means now you are working in the virtual environment. To quit, type `deactivate`. Note now everything you do will only be effective under this particular environment, when you quit, you will not find those installed packages you installed under it.

### Configure Django app

Django official documents talk about how to bootstrap Django app and also connect it to MySQL. Here I talk about how to setup apache to actually serve it. Actually, Django doc has this part too...but I will just summarize it here.

We will be using `mod_wsgi` to host python app with WSGI interface. First to install the module `sudo apt-get install libapache2-mod-wsgi`.

Then, in your own apache site configure file or `default` (in `/etc/apache2/sites-available`), add following for your app:

    WSGIScriptAlias /geodjango /var/www/geodjango/index.wsgi

    Alias /static/ /var/www/geodjango/static/
    <Location "/static/">
       Options -Indexes
    </Location>

As you can see, `/geodjango` will be the address targeting the app. Then create a folder for your project at `/var/www/`, and add a file `/var/www/project/index.wsgi`:

    import os
    import sys
    import site

    # Add the site-packages of the chosen virtualenv to work with
    site.addsitedir('/path/to/your/virtual/env/lib/python2.7/site-packages')

    # Add the app's directory to the PYTHONPATH
    sys.path.append('/path/to/your/virtual/env')
    sys.path.append('/path/to/your/virtual/project')

    os.environ['DJANGO_SETTINGS_MODULE'] = 'projectname.settings'

    # Activate your virtual env
    activate_env=os.path.expanduser("/path/to/your/virtual/env/bin/activate_this.py")
    execfile(activate_env, dict(__file__=activate_env))

    import django.core.handlers.wsgi
    application = django.core.handlers.wsgi.WSGIHandler()

This file acts like the interface to interact with web agents who access the link `geodjango`.

The folder inside of `/var/www` also serves static files for your project. Therefore, change the settings of your django app to point to this directory, and collect static files here.
