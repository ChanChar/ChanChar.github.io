---
layout: post
title: "Deploying Flask on Heroku - Pitfalls"
date: 2015-03-29
backgrounds:
    - https://download.unsplash.com/13/unsplash_523b2af0710a7_1.JPG
thumb: https://download.unsplash.com/15/swirl.JPG
categories: heroku, web development
tags: flask, python, postgresql, sqlalchemy
---


### Goal: Put a Flask based app onto Heroku using SQLAlchemy and Postgresql

Problem #1: The app is using SQLite but Heroku uses something called a Cedar stack which runs an ephemeral file system. Now what does that mean when you use a
file-based system like SQLite?

> Each dyno gets its own ephemeral filesystem, with a fresh copy of the most recently deployed code. During the dyno’s lifetime its running processes can use the
> filesystem as a temporary scratchpad, but no files that are written are visible to processes in any other dyno and any files written will be discarded the moment
> the dyno is stopped or restarted.

A [dyno](https://devcenter.heroku.com/articles/dynos#dynos) is Heroku's form of a linux container that's responsible for running one specific action. When you run an
app on Heroku and manage data using [SQLite](https://www.sqlite.org/about.html), SQLite saves the data to a file (ex. mydata.db). This file lives in a file system
which is cleaned out periodically, resulting in a database that is unable to hold data for more than a [day](https://devcenter.heroku.com/articles/sqlite3).

Problem #2: "Hero- what?" What is Heroku and what in the world is a Procfile?

Heroku is a PaaS or platform as a service. The company provides a cloud environment for users to create and host apps. The goal for many PaaS companies is to reduce the
developer's time in setting up the infrastructure so that he/she can focus on the actual application. Heroku isn't the only cloud platform available, as there are other
companies such as OpenShift, Azure, and App Engine. Which brings us to the next question, what is a Procfile?

> Procfile is a mechanism for declaring what commands are run by your application’s dynos on the Heroku platform.

The definition is straightforward. It is an extra file that needs to be added to my project which helps Heroku know how to manage the app.

Problem #3: How do we pull it all together and have a running app with a running database on Heroku?

My last two weeks have been spent working to create a Flask app with two others. We came across these very same problems when we were ready to deploy to Heroku. This
post mainly serves as a reference for me in case I run into the same problems when deploying another Flask app.

After we were able to get the app successfully running, I backtracked to come up with the following steps. This assumes that you have an organized project, we used
Mitsuhiko's [Flaskr](https://github.com/mitsuhiko/flask/tree/master/examples/flaskr) as reference to organize our own project structures. The project should also have
all the modules needed for your project installed. Of these, [Flask](http://flask.pocoo.org/), [Flask-SQLAlchemy](https://pythonhosted.org/Flask-SQLAlchemy/) and
[Psycopg2](http://initd.org/psycopg/) should be included. I did run into some issues when trying to pip install psycopg2 on my mac (running Yosemite) but
this [post](http://stackoverflow.com/questions/5420789/how-to-install-psycopg2-with-pip-on-python) and similar other posts seem to cover that issue. At this point you
should be able to run your app on a local server usually by calling the run() method on your Flask instance.

We then can create a Procfile. This file is named "Procfile" with no extension. It is case sensitive and it needs to be spelled correctly. It is not "procfile,"
"ProcFile," or "Procfile.py" but simply "Procfile." Then we need to install something called 'gunicorn.' [Gunicorn](http://gunicorn.org/), short for "Green Unicorn,"
which will act as a Python HTTP server when the app is deployed on Heroku. A simple ```pip install gunicorn``` should work. To use gunicorn, we need to specify in our
Procfile so that Heroku knows that we want gunicorn as the server. You can simply add a single line, ```web: gunicorn file_name:app```. In our case we had the
```app.run()``` in a file named "run.py", so our single line was ```web: gunicorn run:app``` which told Heroku to use gunicorn for our web server. We committed
the changes then moved on to using Heroku. [Heroku's toolbelt](https://toolbelt.heroku.com) provides extra commands for the command line to create and manage an app on
Heroku and allows us to use foreman to run the app locally. After installing the toolbelt and logging in using ```heroku login```, we ran
```heroku create <optional_app_name>``` to initialize the app. What you hopefully should see is:

    Creating app-name-###... done, stack is cedar-14
    http://app-name-###.herokuapp.com/ | https://git.heroku.com/app-name-###.git
    Git remote heroku added

If you saw that, your app is now on Heroku. There are two main things to notice, the git address and whether or not the remote git was added. For some reason, one
of the members of the group wasn't automatically having the remote added so he had to manually add the remote using
```git add remote <repo_name> https://git.heroku.com/app-name-###.git```. You can also check the existing remotes using ```git remote -v```. We then made the first
push to Heroku using ```git push <repo_name> master```. At this point, we have an app but no database.

Heroku provides addons which can be added to an app. Postgresql is thankfully offered as addon with a "hobby dev tier" (read: free). To initialize this addon, we ran
```heroku addons:add heroku-postgresql:dev``` which provides you with a postgresql database defined by a COLOR_URL, followed by
```heroku pg:promote HEROKU_POSTGRESQL_COLOR_URL``` to link this database to the app. Furthermore, we needed to set the the value of SQLALCHEMY_DATABASE_API to
```os.environ['DATABASE_URL']```. "DATABASE_URL" is an config variable provided by Heroku when you addon a database. These config variables can be seen in your
app's settings.

Currently the database itself is empty, meaning there are no tables. To create the tables, we needed to go into Heroku's python and import our database instance
to create the tables. Since we used SQLAlchemy, our instance looked like:

    from flask import Flask
    from flask.ext.sqlalchemy import SQLAlchemy

    app = Flask(__name__)
    db = SQLAlchemy(app)

We then boot up Heroku's python terminal using ```Heroku run python``` and run ```from file_containing_db import db```, followed by ```db.create_all()```
to create all the tables that we have defined in our models. To exit the python terminal use, ```exit()```. We noticed that there were a few syntax differences between
SQLite and postgres. For example we found the the ```.like()``` method for queries was different in postgres so we had to change it to ```.ilike()``` which brought
back the intended behavior that we wanted. Finally we have an app with a postgresql database that is functional which can then be further tested and developed.

Here's a list of things to watch out for that I may not have covered above:
1. Make sure the requirements.txt is updated. One of the best ways to keep it updated is to make sure the app runs locally, then run ```pip freeze > requirements.txt```
which will grab all the installed modules and write them into your requirements file. Heroku uses that same requirements file to install the needed dependencies for
your app to run on its server.

2. Turn off debug when running your app. Flask comes built with a debugger but having it on for your deployed app is a security risk and points your users to a debug
page instead of the normal HTTP errors.

3. Make sure Heroku has all the files it needs. You can check which files are in the deployed instance by using Heroku's bash (```heroku run bash```) and using the
```ls``` command. You should see a Procfile, your app file and any other files needed for your app to run.

4. Make sure you're using Heroku's python terminal within your local terminal to set up the database tables once you have connected to Heroku's database.

5. If you have more than one app on Heroku in your account, you may need to specify each action with a ```--app APP_NAME``` tag.
For example, if I wanted to run Heroku's python terminal but had multiple apps on my account, I would need to use ```
heroku run python --app MY_APP``` to run MY_APP's python terminal. Heroku also cancels out a non specific command if there is more than a single app running in your
account.

Resources/References to use:
1. https://devcenter.heroku.com/articles/getting-started-with-python-o
2. https://github.com/yuvadm/heroku-python-skeleton
3. http://postgresapp.com/documentation/configuration-python.html

Overall the experience of reading the docs, learning more about postgresql, sqlalchemy, and setting up Heroku felt like a trial by fire but at the end of the day, I
felt reasonably happy that it all worked out.