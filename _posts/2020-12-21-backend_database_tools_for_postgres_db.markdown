---
layout: post
title:      "Backend Database Tools for Postgres DB"
date:       2020-12-21 21:56:49 +0000
permalink:  backend_database_tools_for_postgres_db
---

For my final portfolio project at The Flatiron School, I developed an app in React/Redux that relied heavily on an extensive database of movies, actors, and the associations between the two models. These are some helpful tools I relied on in my work.

1) psql

I'm working on a Mac, and postgres databases are not quite the same as sqlite databases. They don't live in a visible file in your project directory. They aren't "clickable" to open up in something as simple as DB Browser for SQLite. So the first handy tool is simply psql.

psql is a terminal-based front-end to PostgreSQL. It offers a number of meta-commands and shell-like features to facilitage writing scripts and automating a variety of tasks. For me, simply *seeing* my databases listed was a nice first step. To enter into the psql interface, you'll want to follow these steps:

1. Install or update Homebrew
2. Install or update PostgreSQL
3. Start using the database services with the command `brew services start postgresql`
4. Finally, to enter the psql interface, use the command `psql postgres`

This will give you a prompt that looks something like this: `postgres=#` For your first command, type `\l`, and voila! You're looking at a list of your postgres databases! 

<center>
<a href="https://imgur.com/4qa1AR9"><img src="https://i.imgur.com/4qa1AR9m.png" title="source: imgur.com" /></a>
</center>

2) pgAdmin

Once I could see a list of my named databases, I need a qay to *query* them. My project revolved around finding links between actors and their associated movies, so I knew I'd be messing around with some raw SQL. Enter pgAdmin.

pgAdmin is a free software project released under the PostgreSQL/Artistic license. To use it, download the software from https://www.pgadmin.org/ and open it up on your desktop. (Though it runs in a browser) Follow these steps to access your postgres databases:
1. Click 'create server' in the interface
2. In the 'connection' tab, enter 'PostgreSQL' as the server name
3. In the 'Host name/address' tab, enter 'localhost' 
4. Enter the username and password for the postgres user, and click the 'save' button.

You now have access to your databases! Click on the database you wish to query, and click on the 'query tool'. You can now write some raw SQL to query your tables. This way, I was able to write a search that queried three separate tables and showed me all the movies Keanu Reeves and Laurnence Fishburne had been in together, as well as the characters they played in those movies.  
<center>
<a href="https://imgur.com/mBPhbJT"><img src="https://i.imgur.com/mBPhbJTm.png" title="source: imgur.com" /></a>
</center>

3. Postman

The final tool I found extremely useful was Postman, at https://www.postman.com/downloads/.
Here, you can mimic HTTP requests and immediately see the results in 'pretty' JSON. To do so:
1. Visit https://www.postman.com/downloads/ and click 'Launch Postman'
2. Under 'get started' click, 'create a request'
3. Fill out the request form with the request URL and any required parameters
4. Hit 'send' and immediately see the request results! 

Using Postman I was able to experiment with sending requests to my backend API as I set up new routes and functions inside it. This is how I mimicked my first GET request to link Keanu Reeves and Laurence Fishburne by a movie they'd been in together. 
<center>
<a href="https://imgur.com/XUo6pO8"><img src="https://i.imgur.com/XUo6pO8m.png" title="source: imgur.com" /></a>
</center>

If you're working on a project that requires heavy use of a backend API, these tools should make the job considerably more manageable! 






