---
layout: post
comments: true
title: "Project: SteamScout"
date: 2015-04-12
backgrounds:
    - http://www.hdwallpapers.in/walls/evolve_2015_game-wide.jpg
thumb: https://www.dropbox.com/s/4elfd4cju8fv31d/steam_logo_thumb.png?dl=1
categories: projects
tags: rmotr, flask, postgresql, redis, celery, sqlalchemy
---

Purpose: The prices of games on [Steam](http://store.steampowered.com/), a popular gaming platform, are
constantly fluctuating due to constant sales and Steam's wish list only provides general notifications
for game sales. Steamscout allows users to set "scouts" that watch for a specific price the user is willing
to pay for any given title on Steam. When any game goes on sale and is lower than the price set by the
user, a scout alerts the user via email.

Summary: [Steamscout](http://steamscout.herokuapp.com/) is a flask app that uses the Steam store's json data
and celery to routinely notify users when a specific game drops to the price that the user is willing to pay.
Users have the ability to create an account, search for games, manage price alerts, and receive notifications
via email.

The app uses flask-sqlalchemy/postgresql, bootstrap, and celery/redis (celerybeat).

Overview:
As the final project for Rmotr, students were able to build a Flask app to present on demo day. Although
most of us had little to no experience building apps in Flask, the process of building one as a team has proved
 to be one of the most beneficial steps in our careers as developers.

The team consisted of [Alan J.](https://github.com/AmaxJ), [Martin L.](https://github.com/Tremule), and myself.
Both Alan and Martin have been phenomenal teammates. We were able to coordinate so well regardless of differences
in schedules and timezones. I would be ecstatic to collaborate with them in future projects.

This post serves to be a quick run through of how the project developed over time and to take the time to
review some of the more complicated challenges that went into building this app. One of the first challenges
was figuring out the scope of the project. Questions like "What modules did we want to incorporate?",
"What do we need to further research?", and "How do we structure this app?" were prevalent within the first
few days of the project. Establishing an outline of what we hoped to accomplish within a given time is something
I believe helped tremendously when we actually got to tackling the various components. We presented a very
simple [project scope](https://docs.google.com/document/d/1qt_IZOc579Qe8HO5wrb--vzrk4acdOavfqp7vlH7crw/edit?usp=sharing)
which was used to answer many of the initial questions we had for the app. We had initially set out to create
three pages for basic functionality and had some crude outlines for what our data models would look like. With
quick glance at our research list (features we had no previous experience with), I felt that we had set an
appropriate level of difficulty for our very first app. Based on the urging of our mentors, I could tell that
it may have been a tad out of our current abilities but I felt confident that this team would be able to
deliver.

The first week was mainly spent establishing the base for our project and gave us time to discuss how we would go about
working as a group. We decided to use [cloud9's online ide](https://c9.io/) which would allow us all to have
constant access to the project. In addition, Slack and Google Hangouts made it extremely easy to communicate
throughout the length of the project. By the end of the first week we had a basic version of what we envisioned
to create. We began with a simple project layout; a few files and directories. Having this simplistic base to work
on, we slowly began to integrate the features we wanted. We hit our first hurdle when it came to configuring SQLAlchemy.
We were proficient in working with simple SQL commands and the transition towards using an ORM should have been
theoretically much easier. One of the major factors that we faced in configuring the database was that we
overlooked the need for absolute paths when linking sqlite3 with SQLAlchemy. Eventually we found that that we could use
python's builtin ```os``` library, more specifically, ```basedir = os.path.abspath(os.path.dirname(__file__))
``` and set ```SQLALCHEMY_DATABASE_URI = 'sqlite:///' + os.path.join(basedir, 'appdb.sqlite')``` in the
config file. The variable basedir would help set the absolute path for the database. Once were were able to
create entries/rows in the python interpreter we were able to move on to create forms using WTForms and figure out user
registration. Having some experience with user registration, we were able to create a simple user sign up
and login functionality. We made sure to incorporate security measures such as bcrypt in hashing passwords
and validating the data on the client side and on the server side with the help of WTForms and SQLAlchemy.

In terms of templates and designing our site, we relied on bootstrap and jinja2. Using Bootstrap was a matter
of understanding the 12 column grid system and implementing different components. Fortunately, there were no
real issues using Bootstrap in tandem with jinja2. We did discuss how a mobile first approach would be better
for future projects and that formatting for differing screen sizes (using media queries) would improve the
overall reach and usability of the site. We also took this time to restructure the app. Our app.py file felt
cluttered and it mixed the models, controller logic and routing. Using [Mitsuhiko's tips](https://github.com/mitsuhiko/flask/wiki/Large-app-how-to)
on structuring the files and directories for larger flask apps, we were able to separate our one packed file into
much clearer and organized files suited for our growing app.

The project was coming together quite nicely until we had to deploy to Heroku. In my other [post](http://charleschanlee.me/heroku-flask-setup/)
I described a setback we came across in the transition to using Postgresql instead of sqlite3.

The final challenge was understanding how we could implement scheduled tasks that would refresh the games
table and also routinely check to see if any notifications needed to be sent out. We explored various options
including simple cron jobs and apt-scheduler but eventually settled with celery, more specifically celerybeat.
Many hours were spent reading and scouring celery's docs to better understand how it functioned as a task queue,
and ultimately how we could use celery beat for scheduled tasks. To use celery beat in turn led us to read about
redis and how redis was needed to act as the message broker for workers which would run on Heroku's servers. Admittedly,
the functionality did not seem beginner friendly but it was an integral part of out app's purpose.

By the time for demoday, I felt we had a minimal viable product which went beyond the expectations of our mentors.
Going from essentially no knowledge to implementing user authentications, scheduled tasks, an ORM, hacking together
scripts that would utilize json as a makeshift API, and using Bootstrap felt good. Really good.

There are still many aspects of the app that we can improve on. Making the site mobile friendly and creating more
unittests in addition to improving the methods that interacted with json in the app are on my list of concerns. Overall, I'm glad we were
able to pull through and go beyond our initial expectations and to have the opportunity to work with an awesome
team.


The project can be seen on [Github](https://github.com/rmotr/002-pyp-demoday-g1) and be used on [SteamScout](http://steamscout.herokuapp.com/).
Due to the extended costs of running a worker on Heroku, we sadly had to take out the scheduled tasks after our
presentation. However, a running local instance can be used replicate the same full functionality. Cloning the repo
onto a local environment, installing all the needed modules from requirements.txt and setting up a redis server are
the only things needed to get it up and running.


