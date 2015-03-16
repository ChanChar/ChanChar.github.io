---
layout: post
title: "Crawling Development in Flask"
date: 2015-03-13
backgrounds:
    - https://unsplash.imgix.net/photo-1415889678233-eb900aeee9e1?q=75&fm=jpg&s=a41f4d6b1848cd673323fa4ee17da470
thumb: https://cdn.tutsplus.com/net/uploads/2013/06/flask-preview-400.jpg
categories: general, programming
tags: flask, rmotr, python
---

As we're are working on this Flask project, we began to understand the meaning of the term "microframework." Flask is providing
the bare bones of a potential web app and not much more. Over the last week, we had to address the gaps in our ability
to produce a complete app through intense researching, hours of web searching and lots of "trying out options." We started
out by creating some basic wire frames for each potential page for the site. I have heard of bootstrap and have played around
with it but having to find a way to integrate that into a flask app needed a few more hours of researching and experimenting.
In the end, I was up on a Thursday night at two in the morning smiling at my screen because I had managed to get a base down using
Flask-Bootstrap. Now it wasn't anywhere near complete but I had a new found confidence in using the different components
of Bootstrap. Also having had created plain static sites with a bit of interactivity before, I was able to understand bootstrap's
css/js would add onto the html I wrote to create a sleek, responsive, and simple site. I would need to add some sort of custom
css file to override the default values that bootstrap comes with but overall, the site was beautiful. Routing was a breeze
with Flask's "@app.route()" decorator function and having the different components of the app in different files would help to keep
things organized. We're still very far from producing a minimal viable product to even present to the judges but having the
base of our frontend set up, I feel confident that we can pull through by next week's deadline.

The goal is to have user authentication and models for the database set up by the weekend, pulling it all together in the first
half of the week and finally creating unittests to test the more basic functions of the app by Friday. If it sounds a bit rushed,
it's because it really is. We still need to figure out how to integrate SQLalchemy, OpenID and to use the API within our app. It
really has become crunch time. I'm getting the feeling that most learners get this "make or break" type feeling when he/she is
dumped into this sort of environment. They'll either persevere and push out an app or talk themselves out of finishing the app
saying something about how they were out of their depth.

## Update

The whole process of creating the app feels experimental in nature. We dropped the use of openID and decided on a simpler
system of signing up and logging in. We're using bootstrap to create most of the frontend and it seems to work seamlessly
with Jinja. We somehow got SQLAlchemy to work; the main problem being that we actually didn't know if our database was
being generated for us or that we need to manually create one. It turns out SQLAlchemy will create the database file given
a path within our configuration file. Not too confusing after all. I find it hilarious that it was more difficult for us to
grasp the use of an ORM than to learn vanilla SQL commands.

I believe we're roughly 70% done with our project with a week's worth of time left. This week was overall a win for us after
fighting in the trenches of unknown APIs, authentications and project management.


