---
layout: post
comments: true
title: "Project: QueryBase"
date: 2015-06-24
backgrounds:
    - http://res.cloudinary.com/charliecloud/image/upload/v1435105832/charblog/querybase_bg.jpg
thumb: http://res.cloudinary.com/charliecloud/image/upload/v1434860856/logo_11_lycarl.png
categories: projects, javascript, ruby
tags: rails, backbone
---

The website is live at [QueryBase](http://www.querybase.me). Try it out! If you notice any bugs/errors feel free to let me know by creating a new issue on [github](https://github.com/ChanChar/queryBase).

QueryBase is one of my capstone projects during my time at [App Academy](http://www.appacademy.io/#). It's a Q&A platform made for students, aspiring developers and professionals to discuss computer programming. The motto has been set to "Ask questions, get answers and earn points" which I felt was appropriate for the gamification design of the overall site. I built the app using  Backbone on top of a Rails based backend. Also instead of using [Bootstrap](http://getbootstrap.com/) which I've used in previous projects, I've decided  to try out [Foundation](http://foundation.zurb.com/) which ended up being similar in practice.

You can view the initial project proposal which is also located under the project's [README](https://github.com/ChanChar/queryBase#querybase-project-proposal).

The overall pace of development seemed a bit rushed since I had roughly two weeks to get a MVP project up for review but nonetheless I am proud of what I've built and will continue to improve it. The first week was mainly spent getting the structure of the app down which included creating and organizing the various Rails models and controllers. Which was followed up by creating the appropriate JSON responses (see: API endpoints) for each model with the help of [jbuilder](https://github.com/rails/jbuilder). Jbuilder simply extracts the logic of what to include in the JSON response instead of having that logic within the controller itself. I've stuck to the "skinny controllers" practice not only because it seems to be the 'better practice' but also due to how organized my jbuilder partials are when it came to reusing them within different endpoints.

I've found that creating the basic structure of the app wasn't too difficult. It may have been to the straightforward nature of the app's purpose but possibly also due to how familiar I was with Rails. It was midway through the project where I felt like I had a solid grasp on using Rails for relatively simple to intermediate apps. In terms of using Backbone, after practicing one form of the [Composite View](http://marionettejs.com/docs/marionette.compositeview.html) pattern which is best known in Marionette.js, it was easy to break down the various views (and embedded views). I've taken a huge liking to Composite Views because of how easy it was to separate the various parts within a view into subviews which in turn made it easier to control the logic for all the various parts. It reminded me of the "skinny controller" idea for Rail's controllers in that by having these somewhat uncoupled parts, each view was only responsible for it's own logic and would not have to worry about its subviews beyond attaching them to the template.

There were two significant areas during development that required extra time. The first being translating associations over to Backbone collections and the next incorporating a way to have both infinite scrolling with search capabilities. The first issue was solved by overriding the Backbone model's parse method to create an appropriate collection if a specific key in the JSON response was present. This solution is both clean and short. Now a model has a method that can be used to retrieve an associated collection after it has been fetched. The second problem took a significant amount of time in that my goals were actually impossible with the way pagination and infinite scroll would need to be set up. I was easily able to set up infinite scroll with the help of [Kaminari](https://github.com/amatsuda/kaminari) and a text search was made without much difficulty using Underscore.js' filter method for Backbone collections. Having both caused one main problem. Since a paginated collection would only contain the data for a subset of all the entries, searching through the collection would only return the search results of that subset. The only obvious solution was hitting the database again with a search parameter that would return paginated search results which would allow both those features to coexist.

The best part of building the app was the sense of ownership that came with it. The app in its entirety was planned out, developed and designed by me. The amount of hours put into it, the various questions that came with it and the overall experience has undoubtedly made me a better developer.
