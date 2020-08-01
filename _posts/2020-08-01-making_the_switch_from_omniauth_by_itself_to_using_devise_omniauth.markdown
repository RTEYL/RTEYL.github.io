---
layout: post
title:      "Making the switch from Omniauth by itself  to using Devise/Omniauth"
date:       2020-08-01 17:53:32 -0400
permalink:  making_the_switch_from_omniauth_by_itself_to_using_devise_omniauth
---


I made a rails app that works like a simplified version of angieslist and chose to allow new users to login/signup with GitHub however, `omniauth` proved to be inconsistent with its user authentication and I struggled with configuring it. 

​	Starting with initializing  `omniauth` and `omniauth-github` using `dotenv-rails` to store sensitive keys.

```ruby
#config/initializers/omniauth.rb
Rails.application.config.middleware.use OmniAuth::Builder do
  provider :developer unless Rails.env.production?
  provider :github, ENV['GITHUB_KEY'], ENV['GITHUB_SECRET']
```

configure routes:

```ruby
#config/routes.rb
match '/auth/:provider/callback', to: 'sessions#create', via: [:get, :post]
```

creating  custom login method for users: 

```ruby
#app/controllers/sessions_controller.rb
def auth_loggin
  user = User.find_by(id: auth_hash[:uid], first_name: auth_hash[:info][:nickname])
  user = User.create(id: auth_hash[:uid], first_name: auth_hash[:info][:nickname], password: SecureRandom.uuid) if !user
  
  session[:user_id] = user.id

  if user
  	redirect_to user_path(user)
  else
  	redirect_to login_path
  end
end
```

Each day I struggled to keep authentication working, constantly resetting the database and making new auth tokens

```
$rails db:reset db:migrate
```

after some resets it would work for a few hours and id hit [redirect-uri-mismatch error](https://developer.github.com/apps/managing-oauth-apps/troubleshooting-oauth-app-access-token-request-errors/#redirect-uri-mismatch) repeatedly.  I could also occasionally pass authentication wile GitHub redirects to: ![image-20200801133606506](C:\Users\Tyler\AppData\Roaming\Typora\typora-user-images\image-20200801133606506.png)

I could still go back to my app and a user would be authenticated/found in the database and session but the call back was interrupted by `response status 404`. 

The following day I made the switch to `devise` WITH `omniauth`.

I found this amazing [tutorial](![image-20200801134044565](C:\Users\Tyler\AppData\Roaming\Typora\typora-user-images\image-20200801134044565.png)) to get everything set up initially however I didn’t want to either start from scratch or manually delete old migrations. After running 

```
$rails g devise:install
```

for the initial setup,  I worried about user table creation/migration. Thankfully `devise` already checks for an exiting users table so if you run

```
$rails g devise User
```

`devise` will add a different type of migration to the db:

```ruby
class AddDeviseToUsers < ActiveRecord::Migration[6.0]
  def self.up
    #remove previous email column so devise can do it's thing
    remove_column :users, :email
    change_table :users do |t|
      ## Database authenticatable
      t.string :email,              null: false, default: ""
      t.string :encrypted_password, null: false, default: ""

      ## Recoverable
      t.string   :reset_password_token
      t.datetime :reset_password_sent_at

      ## Rememberable
      t.datetime :remember_created_at

    end

    add_index :users, :email,                unique: true
    add_index :users, :reset_password_token, unique: true
    # add_index :users, :confirmation_token,   unique: true
    # add_index :users, :unlock_token,         unique: true
  end

  def self.down

    raise ActiveRecord::IrreversibleMigration
  end
end
```

Notice the first line of code... `devise` uses `:email` by default to authenticate/find a user and my users table already had an email column setup without the needed indexing so removing it allowed devise to set it up the way it wants.

The following migration adds more ‘devise necessary’  columns to the users table that IS NOT added by default when the users table existed previously.

```ruby
class UpdateUsers < ActiveRecord::Migration[6.0]
  def change
    add_column(:users, :provider, :string, limit: 50, null: false, default: '')
    add_column(:users, :uid, :string, limit: 500, null: false, default: '')
  end
end
```

These columns are used when authenticating users with `omniauth` because the providers `users/auth/:provider/callback` params will also include these fields.

The User model setup was pretty DRY and simple:

```ruby
#app/models/user.rb
class User < ApplicationRecord
  # Include default devise modules. Others available are:
  # :confirmable, :lockable, :timeoutable, :trackable and :omniauthable
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :validatable, :omniauthable, omniauth_providers: [:github]

  def self.create_from_provider_data(provider_data)
    wher(provider: provider_data.provider, uid: provider_data.uid).first_or_create do |user|
      user.email = provider_data.info.email
      user.password = Devise.friendly_token[0, 20]
    end
  end

end
```

 Same goes for an `OmniauthController`:

```ruby
#app/controllers/omniauth_controller.rb
class OmniauthController < ApplicationController

  def github
    @user = User.create_from_provider_data(request.env['omniauth.auth'])
    if @user.persisted?
      sign_in_and_redirect @user
      set_flash_message(:notice, :success, kind: 'Github') if is_navigational_format?
    else
      failure
    end
  end

  def failure
    flash[:error] = 'Ther was a problem signing you in with Github, please try again'
    redirect_to new_user_registration_url
  end

end
```

Removing all the old routes to implement devise routes from this:

```ruby
#config/routes.rb
Rails.application.routes.draw do
  root 'sessions#home'

  get 'signup', to: 'users#new'
  post 'signup', to: 'users#create'
  delete 'logout', to: 'sessions#destroy'
  get 'login', to: 'sessions#new'
  post 'login', to: 'sessions#create'
  match '/auth/:provider/callback', to: 'sessions#create', via: [:get, :post]
```

to:

```ruby
#config/routes.rb
Rails.application.routes.draw do
  devise_for :users
  devise_for :users, controllers: {omniauth_callbacks: 'omniauth'}
```

SO much simpler than `omniauth` stand alone!

Devise also takes care of ALL the views for User sign in, sign up, logout, and edit WITH forms included!!

Next I wanted to increase the use of `devise` across the rest of my app:

```
#app/controllers/omniauth_controller.rb
class OmniauthController < ApplicationController
#becomes 
class OmniauthController < Devise::SessionsController
```

so I can use/override the built in methods

```ruby
 #tells devise where to redirect after sign in
 def after_sign_in_path_for(resource)
    user_path(current_user.id)
  end	
```

This  [SO post](https://stackoverflow.com/questions/13188576/how-to-bypass-password-validation-in-the-following-omniauth-login-method) helped me allow user sign in. and sign up without authenticating an 0auth user. I ended up with:

```ruby
#app/models/user.rb
  with_options if: :validations_required? do |user|
    user.validates :first_name, presence: true
    user.validates :last_name, presence: true
    user.validates :email, uniqueness: true, presence: true
  end

  def validations_required?
    true if provider.blank?
  end
```

Then next area I struggled in dealt with utilizing custom attributes on User creation and edit through `devise` so, with the help of [this SO](https://stackoverflow.com/questions/27541425/rails-devise-allow-custom-fields-in-the-signup-form) question, I created a controller to do just that:

```ruby
#app/controllers/registration_override_controller.rb
class RegistrationOverrideController < Devise::RegistrationsController

  protected
#update without password confirmation when 0auth log in/sign up
  def update_resource(resource, params)
    if !resource.provider.blank?
      resource.update_without_password(params)
    else
      resource.update_with_password(params)
    end
  end

  #custom strong params for my own user attributes
  def account_update_params
     if params[:user].include?(:service_provider)
        user_params
     else
      super
     end
  end
 	def user_params
  	  params.require(:user).permit(:first_name, :last_name, :email, :service_provider, :password, :password_confirmation)
	end

end 
```

and added the controller to the `routes.rb`

```ruby
#config/routes.rb
devise_for :users, controllers: {omniauth_callbacks: 'omniauth', :registrations => 'registration_override'}
```

by overriding `Devise::RegistrationsController` methods I was able to implement my own logic when a User existed without `omniauth` credentials. With the `account_update_params` above, I test for my own user attribute and `if :false` use the devise `account_update_params` instead. In the views:

```erb
#app/views/devise/registrations/edit.html.erb
<% if resource.provider.blank? %>
    <div class="field">
      <%= f.label :password %> <i>(leave blank if you don't want to change it)</i><br />
      <%= f.password_field :password, autocomplete: "new-password" %>
      <% if @minimum_password_length %>
        <br />
        <em><%= @minimum_password_length %> characters minimum</em>
      <% end %>
    </div>

    <div class="field">
      <%= f.label :password_confirmation %><br />
      <%= f.password_field :password_confirmation, autocomplete: "new-password" %>
    </div>

    <div class="field">
      <%= f.label :current_password %> <i>(we need your current password to confirm your changes)</i><br />
      <%= f.password_field :current_password, autocomplete: "current-password" %>
    </div>
  <% end %>
```

For sign up I did the same thing to allow `devise` to create a User without 0auth and with custom attributes:

```ruby
def sign_up_params
  if params[:user].include?(:service_provider)
    user_params
  else
    super
  end
end
def user_params
    params.require(:user).permit(:first_name, :last_name, :email, :service_provider, :password, :password_confirmation)
end
```

and added to the views:

```erb
#app/views/devise/registrations/new.html.erb && #app/views/devise/registrations/edit.html.erb
<div class="field">
  <%= f.label :first_name %><br />
  <%= f.text_field :first_name, autocomplete: "off" %>
</div>

<div class="field">
  <%= f.label :last_name %><br />
  <%= f.text_field :last_name, autocomplete: "off" %>
</div>
```

Finally, to use custom error messages to display with bootstrap, add:

```ruby
 class ApplicationController < ActionController::Base
 helper_method :custom_error_messages
 
 	def custom_error_messages(type, object)
    flash[:"#{type}"] = object.errors.full_messages.join(", ")
  	end
  end
```

and adding this I got from [SO](https://stackoverflow.com/questions/32444342/devise-flash-message-being-notice):

```erb
#app/views/layouts/application.html.erb
<div class="row">
  <div class="col-xs-10 col-xs-offset-1">
    <% flash.each do |type, msg| %>
      <% if type == "notice" %>
        <div class="alert alert-success">
      <% elsif type == "alert" %>
        <div class="alert alert-danger">
      <% else %>
        <div class='alert alert-<%= "#{type}" %>'>
      <% end %>
      <a href="#" class="close" data-dismiss="alert">&#215;</a>
      <ul>
        <li>
          <%= msg %>
        </li>
      </ul>
    <% end %>
  </div>
</div>
```

It took me a few days to convert from vanilla `omniauth` to `devise/omniauth` , with A TON of headaches. It can be done and looking back it seems more simple now..... Through effort, research, and some extra migrations... it worked out well for me and I’m proud of this simple app. My first rails app with 0auth login and multi-user functionality.
