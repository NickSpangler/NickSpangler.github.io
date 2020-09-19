---
layout: post
title:      "MyPlaybillVault"
date:       2020-09-19 02:49:00 +0000
permalink:  myplaybillvault
---


I've built my very first web app! Huzzah!

**MyPlaybillVault** is a way to digitally store, view, and share your collection of Broadway playbills. I've heard over and over "bring what you know to the table," so that's what I've done. And I'm rather pleased with the results! 

I think the secret to my success was aiming big, but starting small. It also helped that I had my idea in my head in the week or two leading up to project week. So as I worked on Sinatra-type lessons as part of my curriculum, I mentally applied it to what I would soon be building. 

Getting started was much easier this time around (compared to my CLI project) thanks to the Corneal gem! With my CLI project, I had to breathe life into every little file in my application, agonizing over how ever file connected to every other file, which gems to require, etc. Corneal certainly made that first hump more palatable, though I did spend some time on Monday morning just aquainting myself with the pre-existing file structure, most notably:  */config/environment.rb*, where my database connection is established,* config.ru*, where my controllers are loaded, and the laybout.rb and main.css files, since I had little experience with HTML and CSS to this point. 

From there I really took it one step at a time. I slowly but meticulously created my models (User and Playbill, Friend and Request came later), built my tables for each model, built my associations using the ActiveRecord macros *has_many* and *belongs_to*, among others, and was finally able to seed my database and drop into tux (another gem!) to start manipulating and playing with actual data.

Comfortable with how my objects interacted, I started straight in on building the seven HTTP requests that would enable my app's CRUD capabilities. 

1. get '/playbills' - to render a user's collection of playbills
2. get '/playbills/new' - to render a form to create a new playbill
3. post '/playbills' - to actually create and save a new playbill (from the form) to the database
4. get '/playbills/:id' - to render an single instance of a playbill to the user
5. get '/playbills/:id/edit' - to render a form to edit the selected playbill
6. patch '/playbills/:id' - to update the selected playbill (from the form) 
7. delete '/playbills/:id' - to delete the selected playbill

Mind you, this was only a starting point. Once I had the *basic* routes figured out I immediately realized the need to expand my controller's functionality with other routes so a user could see a list of their favorites and also see the results of a search to create a new playbill. 

Then it was on to my users controller to build routes that were user-based so a user could:

1. Create an account
2. Log in to their account
3. Log out of their account

And then eventually:

4. Send and receive friend requests
5. See friends' playbill collections
6. Edit their account settings

The project was getting big! Some of my most exciting moments were coming up with an idea for how something would look and function, having no idea how to implement it, and within an hour or two actually learning how to accomplish it, and accomplishing it! I'm talking A LOT of CSS here. 

The largest victory I feel I had (regarding an issue I hadn't even forseen!) was with the way I displayed all the images in my app. At inception, I simply scraped the internet for the images I'd be using, and once scraped I saved the URL of each image to render it again later. But it was pointed out to me by a colleague that the website/server that hosted all these images might be a little... disgruntled to find out I was creating so much traffic for them as my users rendered their playbill collections. 

As I fell asleep on day 4 of my project I wondered if there was a way to actually have my app find an image on the internet, download it to my own server, store it and name it in a dynamic way, thus allowing my users to view locally rendered images, rather than relying on foreign servers.

Surely there must be a way!

There was a way. After a brief bit of searching, another gem to the rescue! I disovered the gem 'down', which accomplishes precisely what I was in need of. Down.download takes an image URL as an argument and turns it into a Tempfile. Add in a `:destination` attribute and I could actually tell it *where* to download to! 

I was about 1/3 of the way there. The issue was that `down` generated a random naming system for the file, but i'd need to dynamically pass on the image's name so that my Playbill object could reference it for later rendering. Enter the ruby Module: FileUtils. Immediately after `down` creates the image Tempfile, I was able to use  FileUtils.mv to dynamically name the image file I was downloading to my server with a title that I first scraped from the image's original webpage, and then paramitized that name. With all this done dynamically, I could then pass that same name (together with the file path) along to my Playbill object to store in the database and call on whenever the user requested to see the image! 

The last hurdle of this process was the nitty gritty details, like passing that name I'd scraped through two sets of HTTP requests, accounting for Playbills having titles of the same show but *different productions* (like revivals) - the answer here was adding in the year of the production to my naming process. 

Put it all together, and in about 2.5 hours I had actually accomplished what I'd only been dreaming about the night before. It was an incredible feeling. 

The big takeway was that once I really have the fundamentals of coding down, i'm going to be able to do anything. Because all the knowledge needed is just a click (or a few...) away. You just have to devote the time and focus to finding the right answer. 

Now it's time to go and use my app!




