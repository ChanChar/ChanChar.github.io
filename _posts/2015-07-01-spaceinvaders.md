---
layout: post
title: "Project: Space Invaders!"
date: 2015-07-02
backgrounds:
    - http://res.cloudinary.com/charliecloud/image/upload/v1435773052/user-profile-images/spaceinvaders-bg_yqvoeu.png
thumb: http://res.cloudinary.com/charliecloud/image/upload/v1435772278/user-profile-images/spaceinvaders-thumb_swzqrf.png
categories: projects, javascript, html5, css
tags: Space Invaders
---

Play it now at [SpaceInvaders](http://charleschanlee.me/SpaceInvadersJS/)! I've decided to keep of list of features to work on at a future date. The list can be found on [github](https://github.com/ChanChar/SpaceInvadersJS).

[![Space Invaders Welcome Screen](http://res.cloudinary.com/charliecloud/image/upload/v1436068077/user-profile-images/Screen_Shot_2015-07-04_at_8.45.50_PM_iljdtu.png)](http://charleschanlee.me/SpaceInvadersJS/)

There were mainly three objectives/challenges while building this game.

1. Use plain vanilla Javascript.
2. Learn more about HTML5 canvas, audio, etc.
3. Have FUN. Seriously, this was a huge factor.

There were so many options in terms of building a game. One option was to make a web version of [Pacapong](http://kingpenguin.itch.io/pacapong). That game is ridiculously awesome. However since there was a time constraint, I decided on focusing on the classics which included space invaders, pong, brick breaker, etc. Space Invaders looked the most appealing and there's that.

Space Invaders was released back in 1978 but it remains to be enjoyable (see: addicting) today. The components of the game break down into 6 main parts which include the aliens, tank, buildings, bullets, movement, and sound. Lets break it down:

Aliens:
One of the first new concepts that I ran into were sprites. Initially, I thought separate images for each individual alien was the common approach but it turns out by using the same image and splicing the desired image piece is more preferred. There is more about the pros and cons of sprites and images in this [SO post](http://stackoverflow.com/questions/1069800/sprites-vs-image-slicing). I was lucky enough to find a site dedicated to keeping the original sprites, sounds, etc online at [ClassicGaming](http://www.classicgaming.cc/classics/spaceinvaders/). By using the sprite sheet, the second thing to learn was how to splice images from a single image. Javascript has a built in [Image()](https://developer.mozilla.org/en-US/docs/Web/API/HTMLImageElement/Image) which generates a given image and also has optional arguments which can be used to specify height and width but that still doesn't really solve the problem of splicing the images at different points. Conveniently, HTML5 allows image splicing (along with actually placing the image on the canvas) using the [drawImage()](http://www.w3schools.com/tags/canvas_drawimage.asp) which provides a lot more options for more refined splicing. Unfortunately there's no easy way (that I know of) that allows you to automatically get the width, height, starting splice coordinates from an image. So by manually finding out the pixels of the various aliens, then rendering them at their starting positions took all of 2 hours. It took me a while to figure out how to actually get the canvas on the page...

So I had the starting positions set, had these immobile alien sprites and no movement. On the sprite sheet there are two of the same colored aliens just with different positions. One alien would have its arm/tentacle/blaster thing in one direction and the other one in another direction. Naturally, I put one and one together to alternate the two images every frame to give the illusion that the aliens were moving. By incrementing the sprites' positions by the largest sprite's width, the sprites would now look like they were moving slowly to the right. Then they continued right until they all disappeared off the canvas. By changing the direction of the sprites when the furthest most right alien hit the edge of the screen to the opposite direction, the aliens would now hit a side and go back to the previous side. In the game, the aliens slowly descend. By simply add to the y position of the sprites, the sprites also now descended every time they hit an edge of the canvas. Their ability to shoot would have to be postponed to a later time.

Tank:
Rendering the tank was the same concept as the aliens except for one minor difference, user control. The player would be able to control the tank using the arrow keys on their keyboard. After using the same process to get the aliens on the canvas for the tank, I looked into getting user input. User input would be processed using Javascript event handlers. By setting an event listeners on keydown, the event itself would contain information about which key was pressed. A quick Google search showed which keys were mapped to the left, right arrows and the space bar (for the tank blaster). I then had to simply check whether or not a key was pressed and move the tank accordingly. The tank 'moved' by just incrementing it's x position by the direction of the arrow pressed, right for positive change, left for negative change. The tank also had the same problem of going off the screen, so taking a lesson from asteroids in class, I made it so that when a tank's position goes off the limits of the canvas to subtract or add the canvas' width so that it would appear on the opposite side.

Buildings:
These were pretty straight forward in that all I had to initially do was render the buildings in relation to the width of the width of the canvas.

Bullets/Shooting:
I think rendering bullets, figuring out how to detect collisions and overall handling of the different interactions was the hardest part of building this game. For the aliens, bullets would have to only come from the last alien from each column (so that they don't end up shooting each other). The tank would have to fire a bullet when the user hits the space bar. There would have to be some sort of function that detects collisions so that when a bullet hit a building, alien or tank, the appropriate reaction would occur. This might be better explained by simply looking through the code itself. Check it out at [github](https://github.com/ChanChar/SpaceInvadersJS). More specifically, the Bullet object and how bullets are created should give a good idea of how the bullets interact with other objects.

Sound:

Background music could be generated using the `<audio>` tag and simply setting the music to loop on end. I may want to have an option to turn off the music since a few people may find it somewhat distracting. Other sounds like the blaster sounds, alien death sounds are easy to implement. By storing the sound in a variable and running the HTML5 `.play()` method, the sound would play when a specific event was called. I did run into one minor hiccup in that if the sound clip was long, the sounds were unable to stack so another quick search revealed by setting the current time of the sound back to 0, sounds would then be reset and ready for the next event.

Summary:

Space Invaders took me a few days from start to finish with most of the time just spent reading how to do specific behaviors. Although, I don't feel like I have a strong grasp on HTML5 canvas yet, I did feel relatively comfortable with the Javascript that was needed. Maybe I'll have to build another game with more complex interactions. Pacapong?
