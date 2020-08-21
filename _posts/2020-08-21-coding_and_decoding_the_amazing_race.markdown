---
layout: post
title:      "Coding (and decoding...) The Amazing Race"
date:       2020-08-21 20:04:05 +0000
permalink:  coding_and_decoding_the_amazing_race
---


It's project week at Flatiron! I've just completed my first Ruby gem, a CLI Amazing Race Emulator!
It involved scraping page after page of https://amazingrace.fandom.com/ and returning that information in a fun and interactive way to the user on their computer's terminal.

The project ended up being slightly bigger than I'd anticipated, but overall I'm very happy with it. The initial setup of a Ruby gem, and the associated GitHub repo were a little difficult, and I'd probably benefit from a review in that area. I have all my files communicating with each other via an `environment.rb` file, but I'm still ess than 100% on using `require` or `require_relative`. I understand the logic behind each, but it doesn't feel like second nature yet. 

My project seemed to boil down to three distinct parts: Scraping, Object Orientation, and the CLI.

**SCRAPING:** The backbone of my project was the amazingrace.fandom website... and I could have done myself a favor and looked elsewhere for content. The website is *great,* don't get me wrong. But the HTML lacks all sorts of helpful identifiers like classes and ids that would have come in very handy as I inspected the race's team pages and episode pages for those elusive bits of text. Ironically, if there's one aspect I spent too much time on, it was probably the scraping, but at the same time, if there's one place I would benefit from spending a little more time on, it's probably the scraping! This was certainly not as "fun" as I expected it to be, but I started to get the hang of it and was able to use simple loops on selectors to grab only the info I needed. Like below, where I needed to iterate over an HTML table full of team names, but also superfluous data, and grab *only* team names for my Episode.teams attribute:

```
doc.css("table")[0].css("td a").find_all do |element|
            if element.text != nil && element.text != "" && element.text != "►"
                episode_attributes[:teams] << element.text
            end
        end
```

**OO Building:** Heading into the project, this is where I actually felt most comfortable. I knew that every SEASON of The Amazing Race *has many* teams, and also *has many* episodes. And each EPISODE can *have many* teams, and each TEAM can *have many* episodes. Once everything was scraped it was just a matter of putting the right data into the right pocket in each class. 

The one place I may have gone astray was in the actual instantiation of all these objects... I wanted it all to happen in the same place, so my CBS class was born (as in Columbia Broadcasting System, on which The Amazing Race is aired). Here, my three important object classes, Season, Team, and Episode are all born into existence, as well as important life cycle events, like #add_team_info and #add_episode_info after a second level of scraping. This is likely not good practice, as instantiation should probably happen... within the class itself? But I am particularly pleased with my CBS.big_bang function which basically calls into existence every object my program uses in a single method call:

```
def self.big_bang(input)
    create_season(input)
    create_teams(input)
    add_team_info
    create_episodes(input)
    add_episode_info
    associate
  end
```

**CLI:** Once all my objects were instantiated and associated with one another, it was just a matter of displaying them to the user in a friendly, easy-to-use interface. As I began my Flatiron education, I'll admit I had little interest in front-end development, or web design. But I must say, after spending *hours* laboring to make my CLI more eye-pleasing, I may have more fondness for it than I anticipated. I used the `colorize` gem to add those trademark Amazing Race colors to my terminal output, and I'm very pleased with how keywords can pop from my menu, so the user easily knows their options to move forward:

```
 def menu
        puts "To see a list of teams, enter '#{"Teams".yellow}'."
        puts "To see a list of episodes, enter '#{"Episodes".yellow}'."
        puts "To pick a new season, enter '#{"Season".yellow}'."
        puts "If you are finished, enter '#{"Exit".red}'."
```

With those three components, I was able to achieve a very accessible way to learn about different seasons of The Amazing Race! 

For a video demo of the gem, visit: https://www.youtube.com/watch?v=c8wqJdfO4SE

To view the GitHub repo, visit: https://github.com/NickSpangler/amazing-race-cli

