---
layout: post
title:      "A simple step by step quide to integrate JWT into a Rails API application."
date:       2021-01-28 17:25:21 -0500
permalink:  a_simple_step_by_step_quide_to_integrate_jwt_into_a_rails_api_application
---


While searching the web and reading docs on GitHub I felt like it can be overcomplicated. This will be a small guide for simple configuration to get you started and be able to further customize from there if need be.

>  NOTE: I’m not using devise but you could add it to your controllers to intercept devise authentication without the `devise-jwt` gem.
>
> Check out [this article](https://dev.to/dhintz89/devise-and-jwt-in-rails-2mlj) for more information with devise configuration

first add the `jwt`gem to your gemfile:

```
gem 'jwt'
```

run `bundle`

Next we’ll add some helpers:

```ruby
#app/controllers/application_controller.rb

class ApplicationController < ActionController::Base
	skip_before_action :verify_authenticity_token
	helper_method :encode_token, :auth_header, :decoded_token, :current_user,
   SECRET = ENV['SECRET'] # your application secret
	 # Rails.application.YOUR_SECRET also works depending on your deployment

  def encode_token(payload) # payload includes your jwt options i.e {user_id, expiration, algorithm}
    JWT.encode(payload, SECRET)
  end

  def auth_header #get token from incoming auth header
    request.headers['Authorization']
  end

  def decoded_token
    if auth_header
      token = auth_header.split(' ')[1] # from `Bearer ${token}`
      begin
        JWT.decode(token, SECRET, true, algorithm: 'HS256')
      rescue JWT::DecodeError
        nil
      end
    end
  end
  
  def current_user # find user from token
    if decoded_token
      user_id = decoded_token[0]['user_id']
      return User.find_by(id: user_id)
    end
  end
end
```

> more customization options can be found on [jwt‘s docs](https://github.com/jwt/ruby-jwt)


```ruby
#app/controllers/registrations_controller.rb

class RegistrationsController < ApplicationController

  def create
    user = User.create(user_params)

    if user.valid?
      # encode token
      token = encode_token({ 
        user_id: user.id,
        exp: (Time.now + 1.day).to_i
        })

      render json: {
        status: :created,
        jwt: token, # add token to json response
        user: UserSerializer.new(user).serializable_hash[:data][:attributes],
        logged_in: true
      }
      else
      render json: {status: 500, errors: user.errors.full_messages}
    end
  end
```

You can follow these same steps for a `sessions_controller` however, for increased security I change the token when a user revisits the site. I set up a `logged_in` route to validate an existing token and return a new one:

```ruby
# app/controllers/sessions_controller.rb

def logged_in
    if current_user # from the helper methods
      token = encode_token({
        user_id: current_user.id,
        exp: (Time.now + 1.day).to_i
        })
      render json: {
        logged_in: true,
        jwt: token,
        user: UserSerializer.new(current_user).serializable_hash[:data][:attributes]
      }
    else
      render json: {user: {}, logged_in: false}
    end
```

When a user logs out, I destroy the token on the frontend which will require login on the next visit to the site. 

For context, I have a `React` frontend with `Redux` and I configure requests using `Fetch` like so:

```js
fetch("https://YOUR-API-URL/login", {
    credentials: "include",
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      Accept: "application/json",
    },
    	body: JSON.stringify(payload)
  })
      .then((resp) => resp.json())
      .then((json) => {
        if (!!json.errors) {
          throw (new Error().message = json.errors);
        } else {
          localStorage.setItem("token", json.jwt); //set token in local storage
          dispatch({ type: "LOG_IN", payload: json });
        }
      })
      .catch((err) => {
        dispatch({ type: "ADD_ERROR", payload: err });
      });
  };
```

However you decide to config log in/ log out the important bits:

```js
// add/remove token from local storage
localStorage.setItem("token", token)
localStorage.removeItem("token");

// include auth header for subsequent requests
const token = localStorage.getItem("token");

fetch("https://YOUR-API-URL/PROTECTED-ROUTE", {
      credentials: "include",
      headers: {
        "Content-Type": "application/json",
        Accept: "application/json",
        Authorization: `Bearer ${token}`,
      },
    })
```


