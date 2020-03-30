---
layout: post
title:      "Creating my first gem!"
date:       2020-03-30 18:05:59 -0400
permalink:  creating_my_first_gem
---

When I started this project it was very intimidating because I didn't have the slightest understanding of gems. sure, I used them a little here and there but, what is a gem? what are gems used for? what's an API? How do I get one? I spent the first few days learning about GitHub and creating a local environment, then the next few searching for an API that was easy enough for a beginner to figure out how to use.

The first API I chose was Marvel [developer API](https://developer.marvel.com/). I created my outline and setup `bundler`, I had it all figured out... right? Nope! I spent two days getting the key and trying to get it to work before I completely restarted my app.  In hindsight, I learned a lot from my failure and even more about API's. Now that I've completed my project I could probably go back to Marvel's API and figure out how to use it but, at the time I was completely overwhelmed by the process.

Let the fun begin! I settled on a similar, albeit simpler, API for [Star Wars](https://swapi.co/api). The most frustrating thing for me about this API is making separate requests for each page number `while i < 10 {response = HTTParty.get("http://swapi.co/api/people/?page=#{i}")}` resulting in longer start-up times. To keep the start-up time down, I needed to separate where new object instances were created. A character is only associated with a species and homeworld/planet when searched for `character.send(#{some_instance_method})` =>`def homeworld {homeworld = API.create_homeworld(@homeworld)}` which decreases start-up time but increases search time.

This particular API uses Imperial metrics for the data I need whereas I use US customary metrics. I found it quite challenging to convert the `mass` and particularly the `height`. Finding the .kg to .lbs conversion ration wasn't difficult but displaying it was a challenge.  Eventually, I found Ruby's `.round` method and was able to return `"#{@mass.round(2).to_i} lbs."`. Finding a way to return `'x'ft. 'y'.in` took me nearly an entire day. I could calculate cm. to ft. or, cm. to in. but not cm to `'x'ft. 'y'in.`. First to convert cm. to in. to ft. `feet = (@height.to_i/2.54)/12` then to get the remainder to in. `inch = feet.remainder(1) * 12` and finally return `"#{feet.to_i} ft. #{inch.round} in."`

Upon reflection, I see that the most difficult part for me wasn't as much about the logic as it was about return values and displaying the correct information. With so many amazing resources at my disposal and such a large and helpful Ruby community, I'm feeling more confident than ever. I restarted my app from scratch twice,  talked to myself for hours and probably spent more time on Google than coding out my app. I continue to struggle and continue to learn and still ever excited for more.
