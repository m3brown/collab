# Collab

[![Build Status](https://travis-ci.org/cfpb/collab.svg)](https://travis-ci.org/cfpb/collab)

## What is Collab?

Collab is a [Django](https://www.djangoproject.com/) project with a standard set of configurations to provide services to reusable apps.

## What is included?

Core Collab comes with:
* Tagging (using a customized version of [django-taggit](https://github.com/alex/django-taggit))
* Notifications
* Search (using [Haystack](http://haystacksearch.org/) and [Elasticsearch](http://www.elasticsearch.org/))
* Migrations (using [South](http://south.readthedocs.org/en/latest/))
* [jQuery](https://jquery.org/)
* Improved Caching


## Setting up a development environment

1. Clone this repo

2. Make sure you have `pip` and `virtualenv` installed:

   ```bash
   easy_install -U pip
   pip install -U pip
   pip install -U virtualenv
   ```

   You may have to prepend these commands with `sudo`, depending on your setup.

3. Create a virtual environment for the project and install the necessary packages:

   ```bash
   virtualenv --no-site-packages --distribute venv    # creates the virtualenv named "venv"
   source venv/bin/activate                           # activates (places you in) the virtualenv
   pip install -r requirements.txt                    # installs main required packages 
   pip install -r requirements-test.txt               # installs packages required for testing
   ```

4. Copy `local_settings_template.py` to `local_settings.py` and edit to match your current 
   environment.
   * Edit `DATABASES` (in the `else` block) to set the database user and password to whatever user you have set up 
     in MySQL (probably `root` with no password, since it's local to your machine).
   * Set a `SECRET KEY`. This can be any string you want.

5. Set up the database:

   ```bash
   mysql -u <user> -e 'create database dbname'
   python ./manage.py syncdb
   python ./manage.py syncdb --noinput --migrate
   python ./manage.py loaddata core/fixtures/core-test-fixtures.json
   ```


   Optionally, you can create random users for local testing.

   ```bash
   python ./manage.py create_users <number of users>
   ```

1. Run the Django server:

   ```bash
   python ./manage.py runserver
   ```

1. Go to <http://localhost:8000> in your browser and log in with user `test1@example.com` and password `1`.

1. You are a winner!


## Testing

Models should have a robust set of unit tests.

When writing unit tests for models, try to separate functionality which needs the DB from functionality which does 
not. Aim to avoid hitting the DB in a test for optimal results.

Views should have unit testing for functional parts of them: that is, try to extract functionality without side effects from the view functions, and then test those extracted functions.

Views should have system testing using [WebTest](http://pypi.python.org/pypi/django-webtest) for page interaction.

