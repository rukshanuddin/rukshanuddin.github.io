---
layout: post
title:      "CLI Project"
date:       2019-12-09 03:39:12 +0000
permalink:  cli_project
---


Today I put the final touches on my first CLI program that I wrote in Ruby.
Here is a link to the GitHub repo!
The project was tough and I didnt know where to start. I thought that I would have it done over break but I didn’t even start. I had no idea what I wanted to do. All I knew was that I had to integrate Nokogiri, scrape 2 levels deep into a website and provide a CLI user interface.
Iended up using Nokogiri, Colorize, Open-URI and Pry to help me on my journey. I ended up settling on making a CLI Pokedex from the Pokemon games. It can definitely be improved, but I just wanted to get my Pokedex to give basic information on the Pokemon and provide a picture if one was available. I was able to extract most of my information from Pokemon Database. Once I knew where I was going to source my information I went and figured out how to get started.
I realized quickly that there were many different editions of Pokedexes and different pokemon within each Pokedex. I figured I would start and create classes for both of these and then I could decide what information that I needed to put in them.
Then I started building a scraper to scrape the links and info for all the Pokedexes. My scraper method scraped https://pokemondb.net/pokedex for the list of all Pokedexes and initialized a new Pokedex object for each. I decided that my Pokedex object needed to store its url, version and info, not all of the Pokedexes had info, so I decided to have three separate pieces of information. Later on, I added an index number so they could be easily selected.
Afterwards, I was able to use the url from the Pokedex object to populate a list of pokemon from that Pokedex. The method I used to populate the list scraped the url passed in from the Pokedex object for pokemon and initialized pokemon objects for each of them. The Pokemon class had number, info, url and name as instance variables for each separate pokemon. I created another scraper to scrape the basic info for each pokemon from the next url that the pokemon object passed in.
For my CLI, i created a nice method for slowly printing out a string to the screen, I named it slow_print. I used colorize to make menus and relied on a lot of recursion to bounce thru menus.
