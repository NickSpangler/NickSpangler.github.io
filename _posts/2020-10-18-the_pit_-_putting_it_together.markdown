---
layout: post
title:      "The PIT - Putting it Together"
date:       2020-10-18 20:18:52 +0000
permalink:  the_pit_-_putting_it_together
---


A note on Rails: I'm not a wholesale fan... 

Believe me, I understand and appreciate the magic of it. It's a really great library of code. Especially with magical functions like `scaffold`, which in essence creates 80% of the functionality required in our Module 2 project with a single command. So I appreciate it. I get it. 

But it still frustrates me! I basically feel like I'm not writing my own code anymore. Instead, I feel like I'm writing code that will *then* write my code for me, and I just have to cross my fingers and hope that it writes what I truly want it to write. So at the end of the day I feel like I need to know* two versions* of the same code: 1) the finished, final version that will do the things I want it to do, and 2) the first, pseudo-version of it that will then *generate* that final version for me. 

A great example of the double-edged sword I'm talking about are all the helpers included in Rails. Because they ARE helpful, for sure. But once you get in pretty deep, and different helpers become more and more similar to one another, it can be a lot. 

A *very* helpful one is the `link_to` method, which saves you a heap of trouble in routing and linking pages to one another. Along with the implicit magic of Rails, `link_to` makes it this easy to generate a link to a user's "show" page.

```
link_to @user.name, @user
```

Simple as that! And we're practically reading in plain English. What do I want? I want a *link to* a specific user's page, so show me the *user's name*, and let's go see that *user*. 

*That* I love. And *that* I got used to. 

But then you get into forms... and the form helpers are copious and varied. Try sorting out the subtlties between `form_tag`, `form_for`, and `form_with`. The most eye-rolling aspect is that the first two are "softly depricated", but still work, should you choose to use them. And you basically need to look up the syntax over and over again to add specific things to your form, or fields in that form. Want to add an id to your form? Look it up. Want to add a class to a field? Look it up. The list goes on. Knowing the HTML isn't enough anymore. You have to know the secret magic code that will *write your HTML for you.* Though I do suppose every aspect of Rails like that will have its supporters and detractors. 

But on to something I really DO like! 

[<iframe src="https://giphy.com/embed/TGWklzt8RZvjV16O8n" width="480" height="480" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/siliconvalleyhbo-TGWklzt8RZvjV16O8n">via GIPHY</a></p>](http://)

One challenge in a Rails app is keeping your controllers "skinny." A lot of logic has to go into your controller actions, but you don't want an overwhelming glut of it. Enter the hierarchy of controllers and helper methods. Because all controllers in your app inherit from the ApplicationController, this is a powerful spot to spawn some code. One of the first things I did after generating my project was to make my ApplicationController look like this:
```
class ApplicationController < ActionController::Base
    before_action :verified_user
    helper_method :current_user

    private

    def verified_user
        redirect_to '/' unless user_is_authenticated
    end

    def user_is_authenticated
        !!current_user
    end

    def current_user
        User.find_by(id: session[:user_id])
    end
end
```

In those 18 lines of code I have now set a default for *every* controller and *every* action to require a logged-in user in order to render that action. This way, instead of starting from scratch and deciding which actions should or should not require a log-in as I go, I know *all* of them require it, and I can just remove that layer of security for the few actions that shouldn't need it. This way, the top of my `User` class can look like this:
```
class UsersController < ApplicationController
    skip_before_action :verified_user, only: [:new, :create]
```
Because obviously a user won't be logged in when they 1) visit the log-in page, or 2) visit the sign up page. This is essentially like throwing a big security blanket over the entire application, and then selectively peeling it back just in the spots that warrant it. 

Throw in other helpful bits of code, like rendering partials and RESTful conventions, and it's pretty easy and exciting to maintain the single-responsibility principle in your project. 

Rails: love it or hate it, it's out there. 


