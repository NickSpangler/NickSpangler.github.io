---
layout: post
title:      "Search Algorithm Speed Boost"
date:       2020-12-30 17:44:19 +0000
permalink:  search_algorithm_speed_boost
---


My Flatiron capstone project is titled Kevin Bacon Bits. 

Demo: https://www.youtube.com/watch?v=UxJodWEFbUk
GitHub: https://github.com/NickSpangler/kevin-bacon-bits

A main feature of the app is a search where a user can enter two actors into search fields and an algorithm will return the 'link' between the two actors via movies and other actors they have each worked with. For example, Keanu Reeves and Laurence Fishburne have *several* one-degree connections from the films they have been in together:

<center>
<a href="https://imgur.com/kgZObgN"><img src="https://i.imgur.com/kgZObgNm.png" title="source: imgur.com" /></a>
<br>
<a href="https://imgur.com/Jgg99MB"><img src="https://i.imgur.com/Jgg99MBm.png" title="source: imgur.com" /></a>
</center>

Obviously the first step of my search algorithm needs to check for the possibility of only one degree of separation between two actors. (i.e.  have they ever been in a film together?)

To check for this link, the pseudo code would be:
1. get all of Keanu's movies
2. get all of Laurence's movies
3. compare the two lists to find matching entries

My first goal was just to get something *functional*, so I went about writing code that mirrored my pseudo code almost exactly. This was my first crack at it:
(in this example, Keanu Reeves is 'target_a' and Laurence Fishburne is 'target_b')
```
        aMovies = target_a.movies
        bMovies = target_b.movies
        matches = []
        aMovies.each{|m| matches << m if bMovies.include?(m)}
```

So in those four lines of code I:
1. create an array of Keanu's movies
2. create an array of Laurnce's movies
3. create an empty array where I will add movies they both have in their lists
4. iterate through Keanu's movies list, checking to see if each one exists in Laurence's movies list
5. if the movie does exist in Laurnence's list, I push it into the 'matches' array, resulting in an array of all the movies they have been in together.

It worked! 

But it's a long slow process... 
<center>
<iframe src="https://giphy.com/embed/3o7buhiXgPU8GmQp4A" width="280" height="280" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/art-3d-3o7buhiXgPU8GmQp4A"></a></p>
</center>

I started doing a deeper dive into ActiveRecord queries to see if I could speed this process up. After all, `target_a.movies` is a built-in method, right? It's not like I had to manally iterate through *every* movie in my database and check to see if target_a was included in the movie's collection of actors. I could just declare it, `target_a.movies` and boom, the list is just there.

I was happy to find that the ampersand operator `&` does exactly what I was trying to do! Use `&` between two arrays, and the result will be only the elements common to both arrays! For example:

```
array_1 = ['cat', 'dog', 'cow']
array_2 = ['eagle', 'dog', 'owl', 'pidgeon', 'cat']
array_3 = array_1 & array_2
```

Can you guess what `array_3` contains? You're right! `array_3 = ['cat', 'dog']` And there was no iteration! No long, slow search process! Using this operator, I turned this:

```
     aMovies = target_a.movies
     bMovies = target_b.movies
     matches = []
     aMovies.each{|m| matches << m if bMovies.include?(m)}
```

into this:

```
     matches = target_a.movies & target_b.movies
```

One line of code, and I had a list of every movie these two actors had been intogether! A small bump in speed, but a bump nonetheless! But the real speed boost was yet to come...

If two actors have NOT been in a movie together, the search becomes much more difficult... I took a breadth-first-search approach and decided that if two actors had not been in the same *film* together, then I needed to see if they'd both worked with the same *actor* together, albeit it in different films. For this step of the search, the pseudo code looks like this:

1. make a list of all the actors target_a has worked with
2. make a list of all the actors target_b has worked with
3. compare the lists and find anyone who both targets have worked with

It sounds simple enough, but where it differs from the first challenge is that in my ORM actors are not already associated with other actors, they are only associated through movies that each actor has been in. Regretfully, this lead me to yet another poorly-performing iterative approach. This was my first stab, which was successful, but slow:

```
 target_a.movies.each do |m|
            m.actors.each do |a|
                levels[:target_a_actors] << a unless a == target_a
            end
 end
				
target_b.movies.each do |m|
            m.actors.each do |a|
                levels[:target_b_actors] << a unless a == target_b
            end
end
				
levels[:target_a_actors].each{ |a| target_c << a if levels[:target_b_actors].include?(a) }
target_c = target_c.first
```

So. Much. Iteration! 
I start with target_a, and I iterate through each of their movies, THEN for each movie I iterate through each of that movie's actors, pushing them into a new array (as long as they aren't my original target_a). I follow the same process for target_b (more iteration!) and then once I have the two lists I can compare them against each other to find which actors target_a and target_b have in common. (In the example above, for the last step I am showing my original iterative approach to the comparison of the two arrays, rather than the better solution I found using the `&` operator)

While this still worked, I was going to hit a scaling problem, because as my search progresses to deeper levels of separation (i.e. four, five, or six degrees...) I the number of actors I would be searching through grows exponentially. If target_a has worked with 100 actors, and *they* have *each* worked with 100 actors... and so on and so forth. By the time my algorithm was searching for a third-degree connection, the search took roughly 24 seconds. Too long for a pleasant user experience.

I dug deeper into the ActiveRecord documentation and searched for a better approach. I spent hours playing with `.where` and `.join` and on and on... but I wasn't getting anywhere. Finally I came across the ActiveRecord method `.find_by_sql`... 

I *loved* our week spent learning SQL, so this looked promising. I opened up pgAdmin and started running SQL queries against my database. If I could figure out the right SQL query to list all the actors a specific actor has worked with, then I could just import that into my Ruby code! It took some fiddling, but eventually I found this:

```
SELECT DISTINCT target_actors.id, target_actors.tmdb_id, target_actors.name, target_actors.profile_path, target_actors.gender, target_actors.created_at, target_actors.updated_at
            FROM
            actors target
            JOIN
            movie_actors target_movies
            ON
            target.id = actor_id
            JOIN
            movies movies
            ON
            movies.id = target_movies.movie_id
            JOIN
            movie_actors rest
            ON
            rest.movie_id = target_movies.movie_id
            JOIN
            actors target_actors
            ON
            target_actors.id = rest.actor_id
            WHERE
            target.id = #{target.id}
```

And just like that, I had a SQL query that immediately returned a list of actors my target actor had worked with. No iteration, just raw, lightning-fast SQL. 

Now I could pull that into ruby, define a class method in my Actor model - `Actor.find_by_sql("sql_statement_from_above")` - and see how much faster the search would run. For a third-degree search I was able to bring what used to be a 24-second process down to a 3-second process - a speed boost of 8X!!!

<center>
<iframe src="https://giphy.com/embed/J3Ao5L98X8oms" width="480" height="272" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/badass-snail-J3Ao5L98X8oms"></a></p>
</center>

I'm sure that the algorithm is still far from optimized, but seeing as I'm a full-stack developer and not a full-blown data scientist, I'll take it for now!
