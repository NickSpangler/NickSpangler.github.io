---
layout: post
title:      "Memeify-Marvel"
date:       2020-11-21 21:46:34 +0000
permalink:  memeify-marvel
---


My last Flatiron project was titled  "The PIT", PIT being short for "Putting It Together". However, I feel like *this* project is way more like a putting-it-together situation! 

Memeify Marvel is a single-page web app, using a rails-API backend, and a JavaScript, HTML, CSS frontend. That's a lot of pieces to pull together! I almost didn't know where to start.  Building off of my experience with creating The PIT, I decided to focus on some front-end design to begin with. With The PIT, I was trying to build *everything* all at once. This meant models, validations, views, etc. It was a stressful way to work! By focusing on front-end design first, I could focus on **one**  aspect (what the user *sees*) and not let my focus waver.

This actually mirrored several of our Module 4 labs, where a fair amount of HTML and CSS were provided for us as a sort of template, and all we had to do was make the "behind-the-scenes" stuff work. (i.e. populate the template they gave us with funcitoning data)  My first step was to do some pencil and paper sketching: 

<center>
<a href="https://imgur.com/eEIU0fu"><img src="https://i.imgur.com/eEIU0fum.png?1" title="source: imgur.com" /></a>
</center>

Then to make it reality! I started building the landing page (i.e. *only* page, given that this is a single-page web app) and had a blast experimenting and learning a lot of CSS tricks on the fly. After a good afternoon of work I had something resembling my sketch, even using some images I'd hard-coded in to represent data users would eventually create.

<center>
<a href="https://imgur.com/6vm4jzt"><img src="https://i.imgur.com/6vm4jztm.png" title="source: imgur.com" /></a>
</center>

Now that I had a way to *display* my the data and imags, I needed actual data and images! I set up a rails-API as my backend. It was one of my first experiences working with an API, but I was familiar enough with rails that generating my models, associations, and controllers wasn't too difficult. Before long I was able to seed my database with a few basic memes and view them in the console. Step two was complete. Data coule be created!

<center>
<a href="https://imgur.com/ueBWGIp"><img src="https://i.imgur.com/ueBWGIpm.png" title="source: imgur.com" /></a>
</center>

So finally I needed an actual interactive frontend, where users could not only *view* memes that had been created, but actually *create* them too! I made a 'template' users could fill in with just a few clicks and some typing for captions. This was a lot of fun, because it finally made the site *interactive!* 

<center>
<a href="https://imgur.com/WFmrnH8"><img src="https://i.imgur.com/WFmrnH8m.png" title="source: imgur.com" /></a>
</center>

One interesting challenge was rendering roughly 400 images for the user to choose from for their memes. I didn't want to hard code all those `<img>` tags into my index.html, so I decided to write myself a little function that would render all the images to the DOM once the main tab had already loaded. 
```
function renderSelectImages(tabName, dirName, count) {
      for (let i = 1; i <= count; i++) {
        tabName.innerHTML += `<div class="square">
                                <img src='Panel Images/${dirName} Panels/${i}.png' class='selectImage'>
                              </div>`
        }
    }
```
This way, once the main tab already loaded, my JS would work in the background to get all those images rendered to the page. I call the method four times, once for each hero available.
```
        renderSelectImages(ironmanTab, 'Iron Man', 146)
        renderSelectImages(thorTab, 'Thor', 92)
        renderSelectImages(hulkTab, 'Hulk', 56)
        renderSelectImages(captainamericaTab, 'Captain America', 102)
```
Even doing it this way, it is *so* many images that it was slowing down the rendering of the memes displayed, located in a completely different tab! So the solution was to utilize the Promise returned by my method that rendered all those memes in the first place, and call a .then() on it with a setTimeout, so that when a user visits the page, the JS prioritizes rendering the completed memes first, waits two seconds, and *then* goes about trying to render those 400 images for users to choose from in other tabs. This setup allows for a much more satisfying user experience. 
```
Api.getAllMemes()
    .then( () => {
      setTimeout(() => {
        renderSelectImages(ironmanTab, 'Iron Man', 146)
        renderSelectImages(thorTab, 'Thor', 92)
        renderSelectImages(hulkTab, 'Hulk', 56)
        renderSelectImages(captainamericaTab, 'Captain America', 102)
      }, 2000);
    })
```

Final steps included adding all my eventListeners, executing .fetch() requests to create memes, retrieve memes, and increment likes for memes. And as a bonus, I utilized the html2canvas library to make memes downloadable to a user's desktop. 

**That** was a journey unto itself, and will be saved for a blog on another day! Happy Meme-ing!
<center>
<a href="https://imgur.com/SpZbVkr"><img src="https://i.imgur.com/SpZbVkrl.png" title="source: imgur.com" /></a></center>



