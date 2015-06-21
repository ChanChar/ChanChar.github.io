---
layout: post
comments: true
title: "App Academy Month Recap I"
date: 2015-06-21
backgrounds:
    - http://res.cloudinary.com/charliecloud/image/upload/v1434916340/charblog/month-recap-bg.jpg
thumb: http://res.cloudinary.com/charliecloud/image/upload/v1434916339/charblog/month-recap-thumb.jpg
categories: app academy, rails, backbone.js, capstone
tags: project, composite views, cloudinary
---

Oh boy. Here we go. One month write up. I am slightly disappointed I haven't kept up
with the one article a week goal but given the circumstances, I would gladly take the time invested
into my final capstone project over a couple articles any time. This recap should cover the past 4 weeks,
the capstone project I've been working on, and my current outlook as of now this far into the program.

Summary: TODO later.

Day: ???

Let's forget about the days and go for concepts. In the past four weeks we have covered significant ground in
both rails and javascript. Shortly after the last post, we were able to experiment with ActionMailer
which would allow us to integrate email support and messaging within our rails app. Now we were able
to send our confirmation emails to ensure that users were using the correct email address in case we would
need to reset their password, troubleshoot bugs/issues, or deal with any general account security. The
concept of dealing with email confirmations is very similar to passwords in that we can compose emails that
would contain a randomized hash that would only be relevant to a specific user. By creating links with the hash
built in, the link could then be sent to the app, verifying the user. Emails continue to be a prevalent
tool today so having that functionality should always be welcomed.

I’ve been on reddit in the past but on Friday, we got to build it! Affectionately called “Reddit on Rails,” the
app was even more practice on authentication and authorization. We began by creating the Users and Sessions
controllers along with a User model and the necessary migrations. At this point, we were all too familiar with
the pieces in auth. Moving forward we created a Sub (for subreddits) and Post models along with their
respective controllers. Each sub and post would require specific authorizations to limit only the moderator or
author to edit/update his/her sub or post.

With the help of form partials, one form could be used in both the creation of new and to update existing subs.
After building the basics and testing out the features, we could then move on to comments. Comments were a bit
tricker in that you could comment on virtual anything (User, Post, Sub). Instead of creating various foreign
keys to track each type of comment (see: DRY), we used a polymorphic association so that it could adapt to any
model given a type and an ID. Now we had simply one table containing all the various comments. Which brought us
to another problem, comments ON comments. This simply required setting an association that pointed to itself
which would result in the nested comments that we wanted. I was able to get a working Reddit clone up and
running, albeit it was rather ugly.

Week: 5

I remember the process of test driven development (TDD). The crawling but extremely stable process of TDD.
This time around, we tackled on behavior driven development (BDD) with integration tests. Unit tests are made to test the specific bit of code to ensure that they’re working correctly while integration tests are to test larger interactions of an app. Integration tests help emulate the use of an app. For example, if the sign in button is clicked, we should be redirected to a sign in page. More on the differences between integration and unit tests on SO.

To write integration tests, we used [Capybara](https://github.com/jnicklas/capybara). Capybara makes it east to set up tests by providing lots of built in functionality to test common elements that would go on a web application including, button pressing, page rendering, form filling, etc. By going through a whole sign up / sign in process with tests, we were positive that our auth system was solid. We also incorporated gems like [factory girl](https://github.com/thoughtbot/factory_girl) and [Faker](https://github.com/stympy/faker) to generate fake users which would be used throughout the tests.

The following two days will be spent creating RailsLite. This two day solo project will help to understand
some of the more common features of rails. The first day was spent setting up a small server to handle
requests, then using those requests to build a response and finally sending out a response. I also got to work
on building the Rails sessions and params methods.

This is an excellent assignment in further helping us understand the framework instead of simply using the
tool. I’m hoping I can finish with most, if not all, of the features and possibly use it in future to build a
small RailsLite project.

On the second day in building up Rails. I worked on the router and route classes which helps process a given
request. The overall process is that a HTTP request is received, the router runs through routes until a
matching url is found, then it creates a new route object and calls the method on the route.

With a bit of regex, the appropriate route can be found fairly easily. Following routes and routers, I moved
onto flashes which work similarly to sessions. It saves a cookie which holds the pending messages for the next
request and the current messages which can be pulled for the current request. Although bummed I wasn't able
to complete all the bonus features, I'll be sure to place this in my todo list. It was honestly a huge
challenge that will not go uncompleted.

No time to spare. Javascript time.

Making the jump to javascript, or should I say ECMAscript. We tackled week one’s problems in JS which included topics like recursion and its form of objects. Most of the learning process seems to come from the ability to understand Ruby, converting that logic into JS syntax. We flew through many of the problems since we’ve encountered them before but also picked up the nuances of JS. Concepts like callbacks, closures, and 'three-quals' were encountered many times over.

Important lesson of the day: Closures are any functions that access variables that were neither passed in or
defined within the function.

Another important point was the idea of declarations vs expressions. Expressions assign a function to a
variable which can be used by calling the variable but a declaration will get hoisted up to the top of a given
scope. One way to escape hoisting is to use named function expressions which use both expression-like and
declaration-like syntax.

Day dos of JS introduced important concepts like ‘this’, callbacks, and the difference between asynchronous
and synchronous functions. Callbacks, in their simplest form, are functions that are passed in as arguments.
These functions are usually called within the function they’re passed into when specific conditions are met.
Which brings us to another problem, it changes how the function is invoked.

Now calling a function should result in the same result right? Nope. An important gotcha is that when we call
a function function-style (ex. object.method), `this` is set to the window instead of what we initially
thought should be the object itself. Whereas when calling a function method-style (object.method()), ensures
that you’re setting `this` properly. We had to constantly check what `this` was in order to get the correct
results.

Last main concept was the idea of asynchronous functions. In Ruby, the order is explicit where the program
will wait for you to input an answer for a given `gets` method and then proceed once that has been fulfilled.
In JS, asynchronous methods will not wait for you, instead it will set callbacks that in the event of a
response (we used the readline’s library for user input), the response could be used. One nice visualization
of how event loops work can be seen using this [tool](http://latentflip.com/loupe/?code=JC5vbignYnV0dG9uJywgJ2NsaWNrJywgZnVuY3Rpb24gb25DbGljaygpIHsKICAgIHNldFRpbWVvdXQoZnVuY3Rpb24gdGltZXIoKSB7CiAgICAgICAgY29uc29sZS5sb2coJ1lvdSBjbGlja2VkIHRoZSBidXR0b24hJyk7ICAgIAogICAgfSwgMjAwMCk7Cn0pOwoKY29uc29sZS5sb2coIkhpISIpOwoKc2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coIkNsaWNrIHRoZSBidXR0b24hIik7Cn0sIDUwMDApOwoKY29uc29sZS5sb2coIldlbGNvbWUgdG8gbG91cGUuIik7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D). Roberts, the creator behind the tool, also gives a nice talk about JS event loops.

The weekend was spent reviewing Javascript. I felt that the transition over from ruby was pretty smooth. I
remembered and alum saying how he wished he would have been more prepared for Javascript, and with that in mind the weekend will definitely be spent over reviewing the concepts of this week and some of the topics that will be covered in the following week.

Week: 6

Remember asteroids? Pew pew boom boom? Well, we built the start for it at the start of week six. This single game would teach us the importance of prototypal inheritance in JS. By creating a general MovingObjects function/’class’, we could also then create the asteroids and ship functions. One important thing to note in JS is that instead of the traditional object oriented inheritance that we were used to in Ruby, we had to use surrogates to link up the prototype chain between the parent class and the child class. This surrogate middleman would make it possible for the child class to inherit the functions defined in the parent class’ prototype but also allow it to define it own methods without affecting the parent class.

This was an awesome intro to what HTML5 and JS has to offer and hopefully over the weekend, I can spend some time sprucing up the asteroids game. I may even want to have this game as one of my profile items to show off to potential employers.

The next day was spent on understanding basic CSS fundamentals. The assignment was to clone a static website that would require knowledge of positioning, psuedo-elements, and specificity rules. Positioning elements on a page was accomplished through the `position` keyword and the offsets could be set by a number of other factors including margin, padding or floats. Psuedo-elements like :before and :after would allow us to input elements without necessarily creating them in HTML. In addition, by using `:hover’, we could change the appearance of a given element on mouse hover.

Specificity rules are about which CSS rules are applied. The general rule is that the more specific a given rule is, the higher precedence it takes. This results in lower precedence rules to not be applied.

Overall, one day of CSS did not make us CSS pros but we will be working on this particular skill when it comes to future projects. In all honesty, I felt like this at the end of the day: ![CSS](http://media.giphy.com/media/5pxnxdzdZfXFK/giphy.gif)

Transiton into JQuery.

JQuery’s model is “write less and do more,” and it does just that. It’s such a powerful library that allowed us to manipulate the elements on a page with a few methods. For example, we could control the result of a click by setting up a click handler using the `on` keyword. This click handler accept a function which would be run when specified elements on the page were clicked. Clickity clackity click.

The projects we tackled on today were building the views for Towers of Hanoi, Tic Tac Toe and Snake which could be played within the browser. It was a needed change of scenery compared to our previous command line games which where usually rendered in black and white. Although today’s goal was to encourage us to JQuery, I still felt more comfortable using JS and saving JQuery as a shortcut or to set up event handlers.

I can see why JQuery is so popular in the JS world. It’s relatively easy and amazing to use. I've heard horror stories where people just completely abandon Javascript and just use JQuery. I hope I dont end up like a JQuery junkie.

Have you ever wanted to customize your web pages, prank your friends, or even make some helpful changes to a web app? Well, you’re in luck because Jquery plugins are just two payments of $9.99. That’s right less than ten dollars a piece.

The day after our JQuery intro was all about JQuery plugins which would simply add to JQuery’s own prototype object and thus make it accessible through any JQuery method. It does seem extremely useful in creating the behaviors for elements just the way you want them to. One difficulty we did run into was implementing transitions which would provide the neat sliding animations of html elements. The main takeaway was to add a class, then remove the class within a timeout so that the class would be removed after a short delay. This delay is long enough so that by removing the class, we end up with the element sliding to where it should have been prior to having the ‘transitioning’ class.

 To get more practice with JQuery plugins, we worked on transforming features of Twitter into JQuery plugins. For example posting a tweet should not refresh the whole page so instead we made it so that, with the help of AJAX, that it would simply append the new tweet to the page once it was posted.

Instead of rendering new pages or being redirected to other pages, we can simply generate elements to show on the page. Other features that were changed include the user search bar, the follow/unfollow button, and how tweets were displayed. I could already see how the curriculum would be easing us into Backbone with all these events.

Week: 7

Backbone is a javascript framework. Although it’s a framework, it seems to better resemble a library with tons of useful features that we simply can use within our app. We built a Pokedex that would use rails as the backend and Backbone on the frontend to delegate many of the various behaviors of the website. We’re currently just scratching the surface by focusing on the models and collections.

By breaking down the various aspects of backbone into groups of features, we should have a solid understanding of how Backbone works by the end of the week.

What I've found was that we took an educational approach in learning Backbone by first using all the antipatterns, then going back and replacing these frowned upon practices with better practices. This has mainly one benefits that I could think of, it helps us better understand which aspects (eg. views) can be cleaned up by Backbone. By seeing the differences in common practices and what we’ve written, I can see why specific patterns are prevalent. I don't think it was the most efficient use of our time but it got the point across.

The day's main topic was about Backbone routers. We converted yesterday’s work into using routers. Overall it was a pleasant experience organizing everything into router events. Nice and organized is something I will always support.

On day 3 of Backbone, we created a journal app. By seeing how all the pieces come together after the past two days, I felt much more comfortable today creating the models, views and router in Backbone. We also touched on the many different types of views including ListView and CompositeView.

The router was key in really making things click. By following the chain of events from the router, it was much easier to understand what exactly was going on. I’m hoping in the next few days, my understanding in using Backbone will develop to the point I can start thinking of how to use it in my final project.

By the next day, Backbone was beginning to feel much more intuitive. By approaching Backbone from a top down perspective starting with the router, it’s easy to follow the chain of events. Today we made a single page RSS aggregate feed that would show the RSS feeds of many different sites. By using app academy’s composite view pattern, we were able to effectively organize the various views as ‘parent’ views and their subviews. By having subviews, a lot of the functionality of a single view could be modular in that we didn’t end up with views that contained more than enough methods.

Although we really only have the basics down, we did cover many interesting topics including zombie/ghost views, view patterns and setting up rails to interact with backbone. Tomorrow we will tackle the popular site, Trello, on our own using almost everything we’ve learned in the past few weeks. Then it’s off to final projects!

We’re coming towards the finish line and to wrap things up, our last class assignment was spent building a clone of Trello. This will be a primer in preparing us for creating our final projects. The app uses Rails in the backend and Backbone for the frontend. In the two days that we’re given to complete the project, we will review concepts like using jbuilder to build out a response that can be used by backbone, using third party libraries like jquery ui for customized behaviors like dragging and dropping elements, and being able to build a minimum viable product.

Although the project looks simple, there does seem to be multiple complex parts that work together to produce the app.

 It was day 2 of Trello, well technically day 2.5 for me. I spent Friday night to continue working on it. Man, oh man. I find backbone to be ridiculously fun. When you’re given the tools to organize the behavior of website, I am going to take that and run with it. Backbone has allowed us to organize our views, the various event listeners, and has allowed us to develop much more complicated interactions with Rails.

I was close to finishing up a minimal viable product that would have user sign up  and log in, draggable boards, cards and lists. Playing around with jquery ui was a neat experience that reminded me of bootstrap. Simply adding classes to elements would allow jquery ui to generate interactions on the site. I am going to continue to polish this project and hope to be able to showcase it at a later date.

Week: 8

The time to work on our final projects is here. I’m creating a StackOverflow inspired clone that will hopefully showcase my abilities as a developer. It’s hands down one of the most fun things I’ve set out to do. To create an entire app using everything we’ve learned will not only be fulfilling but also give me time to experiment with other aspects of web dev that I am interested in. Phase 1 for me was simply creating user auth, the rails models and controllers for the Questions, Answers and Comments.

Overall the project may seem simple but I’m hoping to incorporate a few of the more difficult backbone and rails patterns to create features that I think a Q&A board should have. It’s going to be a great two weeks of building.

Early in the week, I had most of the models that I needed for the rest of the project including Questions, Answers, and Comments. I ran into a hilarious (see: sarcasm) bug which was logging out a user from a Backbone view. I made the correct ajax request and have everything set up correctly except for the habit of `preventDefault` any button/form related behaviors in my Backbone views. This would log out the user but not redirect them to the login page threw me for a loop. A silly loop.

I’m hoping to finish the bulk of the functionality including, view counts, a point system, and badges by the end of this week. Eric gave a talk about like buttons and how to incorporate them into our projects, I may spin that to fit a votes model.

I’m ahead of schedule. I’ve finished a good amount of the project but I hit a major snag on the voting system. Votes are a polymorphic in that both Questions and Answers have votes. One way to go about it was to make a join table for just the votes that would contain a value (+1, -1) to signify an upvote or a downvote, the votable_id, the votable_type and the voter_id. The main problem I had was understanding how to replicate the association on the Backbone end of things. By using jbuilder to send the response containing the number of total votes and whether or not the current user has voted on that particular item seems to cover everything I need to display votes. However, the create and delete actions of a vote are much more complicated in that I have to account to several different cases that could happen. If a user has upvoted before by clicking upvote again should destroy the initial vote, increment down the total votes, and reflect that on the backbone side by re-rendering the voting composite view.

I’m finding that I have a ton of composite views just to break down the multiple elements for the page itself. Much more complicated than I initially anticipated.

On the third day progress hit an all time slow since all the features that I had to work on were the nitty gritty details. My web app has gotten to a medium sized app where there’s a good amount of functionality. Having chosen to do Stackoverflow like project, I thought it would be simpler but the deeper I get into the project, it’s become much more complex. Different parts of the app need to be organized in a way so that I wouldn't screw myself over when it comes to refactoring. I tried to loosely couple the different parts but it came to a point where many parts were interacting with one another so that I had no real choice but to somewhat link behaviors to one another.

I felt that I did a decent job with the CSS over the past weekend so I feel comfortable about the way it’s presented. It’s all about getting those last few features in and I should be good to go. I may even end up continuing to add features until I’m completely satisfied about the number of features it has.

We’re beginning to wrap up with final projects. Tomorrow we have a pair code reviews so getting our MVP finished by today was vital. I finished up completing the CRUD features for all my models and added some CSS to make the UI a bit smoother. I’m hoping to implement two more features including infinite scrolling and possibly a landing page for new users.

I have yet to implement a better badge system so until I get that worked out it looks like that has to be delayed. Overall the project is going well and I’m going to have to put some time in it past the soft deadline for next week.

Today was peer review day for our final projects. My project got fairly nice reviews with some very good suggestions. I made sure to take down notes on what my reviewers explained that they would have liked to see.

Taking these suggestions, I’ll be working them into my project over the next few days up till about Tuesday, the due date. I did run into a major hurdle in that I worked on infinite scroll for the whole day and may not even end up using it due to how it messes with the search functionality. Although there are ways to salvage this, I may decide to delay finishing up the scrolling feature until after fixing all the other smaller features that I wanted to work on.

Two main things we’re accomplished on the Friday before next week's due date, user seed data and fixing infinite scroll + search functionality. In looking for seed data, I wrote a simple and short script that would pull users from a site that contained usernames and profile pictures of users that gave their consent for use in production sites. It was interesting to use bash commands within ruby simple by placing the command within backticks.

The other major accomplishment was working through how to set up infinite scroll in junction with the search. I’ve mainly taken two things away from the struggle of trying to understand how to go about implementing both. The first being that the database will eventually have to be hit. I had originally fetch the whole collection which would give me the freedom to search through the collection without having to hit the database again but with infinite scroll placing a limit to returned items per fetch, I could no longer simply search through fetched parts. The second being was that specialized methods can be placed directly on the collection.Having the ingrained idea of only having specific methods such as `getOrFetch` in the collection file, it hadn’t dawned on me that I was able to put any method in there. By placing the search method on the collection itself, I would be able to pass search params which would fetch paginated search results.

Projects are due Tuesday so the final touches will take place over the weekend and in the last two days.

The project will be hosted on [queryBase.herokuapp.com](queryBase.herokuapp.com) or possibly an actual
non-heroku domain name. Currently it should be in maintenance mode but once I've made a few changes it should be live for all to see.

That's a wrap. One month's worth of post in less than 30 minutes of reading time.  
I'm hoping my next post will shed some light on how I feel in my progress in becoming a developer. I'll also be sure to share some of the awesome libraries that I hope to incorporate into my project.

Note: Cloudinary is awesome of image/video hosting. My site feel faster. No more redirects for images for me. YESSSSSSSSSSSS.

Note 2: I love CompositeViews.
