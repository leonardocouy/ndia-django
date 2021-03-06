ndia-django
===========

Northside Digital Innovation Alliance (NDIA) project prototype in Django

The Django portion of the NDIA project aims to build a backend to manage and
provide information about public technology centers, classes, and events in
North Minneapolis. The backend will include an API endpoint that enables API
clients to query the above information. The backend will also provide a UI for
administrators to edit and manage the above information.

[ndia-angular](https://github.com/OpenTwinCities/ndia-angular) is intended to
be a client of this backend.

This backend is deployed online at <http://ndia.herokuapp.com/>. See 
<http://ndia.herokuapp.com/api/events/?format=json> for example output.

Dependencies
============

##Python

Make sure you have Python on your computer. The following two commands should work:

```
python --version
pip --version
```

## Recommended - virtualenv and virtualenvwrapper

[Virtualenv](https://virtualenv.readthedocs.org/en/latest/) makes it possible 
to install multiple Python environments on one computer; including installing a 
Python environment just for this project. It is not required to run this, but
if you're working on multiple Python projects, you should probably install this.

[virtualenvwrapper](https://virtualenvwrapper.readthedocs.org/en/latest/) 
provides commands and organization for using multiple virtualenvs.

Instructions in this README assume that virtualenv and virtualenvwrapper are
installed and make use of those tools.

### Install

```
pip install virtualenv
pip install virtualenvwrapper
```

Install Northside Digital Assets System Backend
===============================================

## Get Northside Digital Assets System Backend

```
git clone https://github.com/OpenTwinCities/ndia-django.git
cd ndia-django
mkvirtualenv ndia-djagno
pip install -r requirements.txt
```

## Prep the Database

By default, the Northside Digital Assets System Backend will use a SQLite 
database named *ndia.db*, which requires no configuration for connection. See
**Configure Northside Digital Assets System Database** below if you would like
to use a PostgreSQL database instead.

Regradless of which database you use, you will need to run database migrations
to prepare the database.

```
cd ndia-django/ndiaDjango
python manage.py migrate
```

## Create a Superuser

In order to login to the backend's UI and add data, you're going to need a
superuser in the database. To create one, simply run

```
cd ndia-django/ndiaDjango
python manage.py createsuperuser
```


Run Northside Digital Assets System
===================================

```
cd ndia-django/ndiaDjango
workon ndia-django
python manage.py runserver
```


See the Northside Digital Assets System
=======================================

## Manage the data

Visit `http://localhost:8000/admin/` and enter the username and password
created above. Then, use the UI to add an Organization, then an Event.

## Query the API

Visit `http://localhost:8000/api/events/?format=json`

Update the Northside Digital Assets System
==========================================

```
cd ndia-django
git pull
ndia-django
pip install -r requirements.txt
cd ndiaDjango
python manage.py migrate
```

Configure Northside Digital Assets System Database
==================================================

This section will help you setup the Northside Digital Asset System to use
a PostgreSQL database.

## Install PostgreSQL

Obviously, you'll need to have PostgreSQL installed and running. So 
[download and install it](http://www.postgresql.org/download/).

## Configure

First, create a user and database in PostgreSQL

```
sudo su postgres
createuser ndia -W
createdb ndia
```

We also need to update PostgreSQL to allow for password based authentication of
the user that we just created. Edit the PostgreSQL Authentication Configuration
File (located at /etc/postgresql/<version>/main/pg_hba.conf in Debian) and
add the following to the file immediately after `# "local" is for Unix domain 
socket connections only`:

```
local   all             ndia                                    trust
```

Save and exit. Now restart PostgreSQL

```
sudo service postgresql restart
```

Next, configure the Northside Digital Assets System to use what we just 
created.

```
cd ndiaDjango/ndiaDjango
cp config.py.sample config.py
```

Now edit settings.py with your favorite editor. If you have a default 
installation of PostgreSQL on your local machine, then you do not need to change
the host or port settings. 

Possible Reading
==============

- [A Rest API using Django and authentication with OAuth2 AND third parties!](http://httplambda.com/a-rest-api-with-django-and-oauthw-authentication/)
