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

Problem #1: The app was using SQLite and now needs to instead use postgresql since Heroku uses something called a
Cedar stack based on a ephemeral file system. Heroku defines their ephemeral file system as the following:

> Each dyno gets its own ephemeral filesystem, with a fresh copy of the most recently deployed code. During the dyno’s
> lifetime its running processes can use the filesystem as a temporary scratchpad, but no files that are written are
> visible to processes in any other dyno and any files written will be discarded the moment the dyno is stopped or
> restarted.

A dyno is Heroku's form of a process container; it's some form of space and function that lets you run your app on their
server. When you want to run an app on heroku, [sqlite](https://www.sqlite.org/about.html) saves to a file (ex. mydata.db)
which is then placed on a dyno. Since sqlite relies on a file, which is being run on a "temporary scratchpad," the data
that is being written to the file will be discarded once your app is stopped or restarted. This results in database that
is unable to hold data for more than a [day](https://devcenter.heroku.com/articles/sqlite3).

Problem #2: "Heru- what?" The app now needs to be updated so that it can be used with Heroku. What in the world is a Procfile
and where does it go? Heroku defined a [procfile](https://devcenter.heroku.com/articles/procfile) as:

> Procfile is a mechanism for declaring what commands are run by your application’s dynos on the Heroku platform.

The definition was pretty straightforward. It was an extra file that needed to be added somewhere in my project that would
help Heroku know how to set up my app.

Problem #3: How do we pull it all together and have a running app with an available database that would be able to
hold data for more than one day.

I came across the same problem with the group flask app that I've been working on. We were ready to deploy to Heroku but
haven't set up any of the requirements needed before deploying. This post really serves as a reference for me in case I run
into the same problems when deploying another Flask app to Heroku.

After we were able to get the app running, I backtracked what we did and came up with these steps. This assumes that you
have already set up an organized project, we used Mitsuhiko's [Flaskr](https://github.com/mitsuhiko/flask/tree/master/examples/flaskr)
as reference in organizing our project directory. It also assumes you have all the dependencies already installed which should
include [Flask](http://flask.pocoo.org/), [Flask-SQLAlchemy](https://pythonhosted.org/Flask-SQLAlchemy/) and [Psycopg2](http://initd.org/psycopg/).
I did run into some issues when trying to pip install psycopg2 on my mac (running Yosemite) but this [post](http://stackoverflow.com/questions/5420789/how-to-install-psycopg2-with-pip-on-python)
and similar other posts seem to cover that issue. At this point you should be able to run it on a local server usually by
calling the run() method on your Flask instance. This was in another separate file for my group in run.py but it shouldn't matter
as long as you have
```app_name.run()```
somewhere on a file.

We then can create a Procfile. This file is just plain "Procfile" with no extension. It is case sensitive and it
needs to be spelled correctly. It is not "procfile," "ProcFile," or "Procfile.py" but simply "Procfile." Then we need to install something called
'gunicorn.' [Gunicorn](http://gunicorn.org/), apparently short for "Green Unicorn" will be the Python HTTP server on Heroku.
A simple ```pip install gunicorn``` should work. Now we can use gunicorn inside our Heroku app but first we need to specify in our
Procfile so that Heroku knows that we want gunicorn as the server. You can simply add a single line,
```web: gunicorn file_name:app```
In our case we had the ```app.run()``` in a file named "run.py", so our single
line was ```web: gunicorn run:app``` which told Heroku to use gunicorn for our web server. We committed the changes then moved on to
using Heroku. [Heroku's toolbelt](https://toolbelt.heroku.com) provides extra commands for the terminal to create and manage an app on Heroku,
use foreman to run the app locally, and allows us to use Heroku's git. After installing the toolbelt and logging in
using ```heroku login```, we can move into the root of the project directory and use
```heroku create <optional_app_name>``` to initialize the app. What you hopefully should see is:

    Creating app-name-###... done, stack is cedar-14
    http://app-name-###.herokuapp.com/ | https://git.heroku.com/app-name-###.git
    Git remote heroku added

If you saw that, your app is now on Heroku. There are two main things to notice, the git address and whether or not the remote
git was added. For some reason, one of the members of the group wasn't automatically having the remote added so he had to manually add the
remote using ```git add remote <repo_name> https://git.heroku.com/app-name-###.git```. Then we can make the first push
to Heroku using ```git push <repo_name> master```. At this point, we had an app but no database.

First change we had to do was change the value of SQLALCHEMY_DATABASE_API so that it works with Heroku's postgresql. Oh wait,
we need to first add the database to our deployed app. Thankfully Heroku has a free tier add on that most people can use. We ran
```heroku addons:add heroku-postgresql:dev``` which provides you with a postgresql database defined by a COLOR_URL. Then we want to link
our app with the database using ```heroku pg:promote HEROKU_POSTGRESQL_COLOR_URL```. It was simply that easy to add a database.

Now the database itself is empty, meaning there are no tables. To create the tables, you would need to manually go into
Heroku's python and import your database instance and create the tables. Since we used SQLAlchemy, our instance looked like:

    from flask import Flask
    from flask.ext.sqlalchemy import SQLAlchemy

    app = Flask(__name__)
    db = SQLAlchemy(app)

We could then use ```Heroku run python``` and run ```from file_containing_db import db```, followed by ```db.create_all()```
which would create all the tables (models) that we have set. To exit the python terminal use, ```exit()```. We noticed that there
were a few syntax differences between SQLite and postgres. For example we found the the ```.like()``` method for queries
was not functioning the same way as when we were using SQLite so we had to change it to ```.ilike()``` which brought back the
intended outcome that we wanted. At this point we have an app with a postgresql database (with a few bugs) that is functional for
further testing and developing.

Here's a list of things to watch out for that I may not have covered above:
1. Make sure the requirements.txt is updated. One of the best ways to keep it updated is to make sure the app runs locally,
then run ```pip freeze > requirements.txt``` which will grab all the installed modules and write them into your requirements file.
Heroku uses that same requirements file to install the needed dependencies for your app to run on its server.

2. Turn off debug when running your app. Flask comes built with a debugger but having it on for your deployed app is a security
risk and points your users to a debug page instead of the normal HTTP errors.

3. Make sure Heroku has all the files it needs. You can check which files are in the deployed instance by using Heroku's
bash (```heroku run bash```) and using the ```ls``` command. You should see a Procfile, your app file and any other files
needed for your app to run.

4. Make sure you're using Heroku's terminal and not your local terminal to set up the database tables once you have connected
to Heroku's database.

5. If you have more than one app on Heroku in your account, you may need to specify each action with a ```--app APP_NAME``` tag.
For example, if I wanted to run Heroku's python terminal but had multiple apps on my account, I would need to use ```
heroku run python --app MY_APP``` to run that app's python terminal.

Overall the experience of reading the docs, learning more about postgresql, sqlalchemy, and setting up Heroku felt
like a trial by fire but at the end of the day, I feel reasonably happy that it all worked out.