---
layout: post
title:      "Project Planning is so important!"
date:       2020-09-25 21:55:03 +0000
permalink:  project_planning_is_so_important
---

Up to this point in the course we’ve been creating applications using either JavaScript front end OR ruby/rails backend. In my last project I found out (the hard way) pre-project planning to be the greatest resource in development. Right before I started typing this blog I went back and looked at my original pseudo code to see how much of my project ‘ran away’ from me. 

Initially I like to make an object with keys and values, my object representing the app I’m trying to build with each key indicating a functionality of the app and its values to nest more functionality specific its parent. I started from:

```
memory game

frontend {
	index:{
		fetch leaderboards
		create a user
		game instructions
		play -> start game
		difficulty level?
	}
	
	play{
    -highlight a random box store in array
    -listen for click event on box add to clicked array

    -check if highlighted box matches event box in arrays

    -highlight previous box + new random box and add to 					array

    -listen for click event(s) WHILE checking array (if 					first click doesn’t immediately match -> failure .					..etc...)

    -add to score on each success + previous successes(first 				round = 1, second round = 2 etc...)

    -upon failure display results and score ranking

    -update user info and leaderboard

  }

}

backend{
	user: username, score
	leaderboard: has many users, sorted by user.score

}
```

While my code doesn't resemble this at all, it sums up the functionality of each aspect of the app. when the game starts `index:{}` implement these tasks:

```
fetch leaderboards
create a user
game instructions
play -> start game
difficulty level?
```

it had been a number of days since I last looked at the original pseudo but, having built the app initially with this in mind, it paved the way for me to build it out. The last project was a backend Ruby on Rails project with 0auth and I had struggled so much getting it off the ground because I didn’t spend adequate time with planning each resource. I found myself going back and forth on what to do next, what to remove vs what I've already implemented, and it made the project more stressful. The bottom line... Planning is KEY to productivity. I knew exactly what I wanted to be able to do and I dedicated an entire day coding possible functions, classes, eventListeners, etc.. just testing the app flow before ever creating a repo.

The day spent pseudo coding brought more than just planning to the table. I found gaps in understanding and knowledge that I typed up to research and have a reference of better practices to implement. Additionally, I saved the dates and URLs that lead to better understanding or issue resolution. I also started taking notes (with screen shots) at the end of each day to remind myself of the exact issues I last faced.

In conclusion, Flatiron does a great job with not only teaching and practice but, I’m given advise on best practices to give me what I need to succeed. I now use the many resources at my disposal to prepare myself and my code in better ways. Knowing how and what to research, how to plan and project, and how to start an app efficiently and effectively. 
