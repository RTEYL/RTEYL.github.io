---
layout: post
title:      "My First Sinatra App"
date:       2020-05-29 04:02:51 +0000
permalink:  my_first_sinatra_app
---


​	I felt my understanding of ruby and Sinatra had grown so much but again I was a little overwhelmed about how little I knew of the app structure used in Sinatra applications. I can't describe what it felt like to open a blank document and form an app structure. Outside of the curriculum I never spent time creating my own validations either which, led to more blank spots in my understanding of Sinatra. I'd still say its easier to make a small rails app than the previous projects api gem.

When thinking about the app structure I felt that it was easier to build a rough draft similar to [corneal's](https://github.com/thebrianemory/corneal)

```
├── config.ru
├── Gemfile
├── Gemfile.lock
├── Rakefile
├── README
├── app
│   ├── controllers
│   │   └── application_controller.rb
│   ├── models
│   └── views
│       ├── layout.erb
│       └── welcome.erb
├── config
│   ├── initializers
│   └── environment.rb
├── db
│   └── migrate
├── lib
│   └── .gitkeep
└── public
|   ├── images
|   ├── javascripts
|   └── stylesheets
|       └── main.css
└── spec
    ├── application_controller_spec.rb
    └── spec_helper.rb
```

I typed out: 

```
user
	has_many collections
	validates username && password
		vars{
		username
		password
		about
		}
collection
	belongs_to user
	has_many items
		vars{
		name
		description
		items
		}
item
	belongs_to collection
	validates image? # if possible
		vars{
		name
		condition
		price? #maybe? maybe not..
		image? #if i can validate
		}	
```

Typing my app out in sudo will now be apart of every project I do. This notepad.md took 5mins to make and I used it constantly until I finalized my migrations AND completed my app structure (at least a few days worth of coding). However, thanks to the amazing gem that [corneal](https://github.com/thebrianemory/corneal) is, I had the basics down pretty quickly. I was also able to use a few great templates to get my CONTRIBUTING.md (see [here](https://gist.github.com/PurpleBooth/b24679402957c63ec426)), LICENSE.md (see [here](https://help.github.com/en/github/building-a-strong-community/adding-a-license-to-a-repository)) and README.md(see [here](https://www.makeareadme.com/)) to exponentially save time while following [GitHub’s](https://help.github.com/en/github/site-policy/github-community-guidelines) community guidelines.

​	One of the biggest challenges I faced with this project is that it can be difficult to find help and articles for coding with Sinatra. nearly all questions I had can be answered with Ruby On Rails but not necessarily with Sinatra. For example, I needed to validate a user input for an image URL to make sure a user can only input specific types of images. The first gem I came across looks amazing and does everything I need but I couldn’t find any docs for [Carrierwave](https://github.com/carrierwaveuploader/carrierwave) that helped with the use of Sinatra. To be fair I don’t know a lot about rails currently or to ‘convert’ the coding/terminology to Sinatra. [Paperclip](https://github.com/thoughtbot/paperclip) seemed a little advanced (though it did have non-rails instruction) it also did a lot more than needed for my small project. I stumbled on this [Stack Overflow](https://stackoverflow.com/questions/7091816/rails-validation-of-image-simple-form) article for a simple validation regex: 

```ruby
validates :image_url, allow_blank: true, format: {
  with: %r{\.gif|jpg|png}i, #I added an |jepg| for and extra validation
  message: 'must be a url for gif, jpg, or png image.'
}
```



I also spent some time figuring out why my servers were dying and how `rackup` and `shotgun` run local servers. I noticed that (after quite some time I should add) whenever I would accidentally force stop my server `ctrl^z` or reset/restart my database I would get `Input/output error @ io_writev - <STDERR>`. Finding out how ports work and use processes was a game changer. If you look hard enough at the task manager you'll see a suspended Ruby process ![Untitled](D:\Tyler\Desktop\Untitled.jpg)that is holding up that port so restarting the server without closing the process a new one cannot startup. 


