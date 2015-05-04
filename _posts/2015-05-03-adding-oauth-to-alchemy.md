---
layout: post
title:  "Adding OAuth to Alchemy CMS"
date:   2015-05-03
summary: We needed to allow some users to login with Facebook, this is how we did it
categories: ruby rails alchemy cms oauth omniauth
---

We have a project at work built on the Alchemy CMS. One of our needs for the
project is to allow users sign in at the "member" level using third party
OAuth credentials from companies like Facebook and Google. Because we are also
using the `alchemy-devise` gem, using Omniauth seemed like a natural choice, but
it took a little effort to get it working.

The `alchemy-devise` gem provides a lot of setup and things that you get from
devise already. It is part of the `Alchemy::User` model. You just need to add a
couple more columns to track the `uid` and name of the OAuth `provider`. The
migration looks like this:

{% highlight ruby %}
# db/migrate/..._add_columns_to_alchemy_users.rb
class AddColumnsToAlchemyUsers < ActiveRecord::Migration
  def change
    add_column :alchemy_users, :uid, :string
    add_column :alchemy_users, :provider, :string
  end
end
{% endhighlight %}

Next, add OAuth providers. This example uses Facebook and Google, but there are
many [provider strategies](https://github.com/intridea/omniauth/wiki/List-of-Strategies)
that you can choose from.

{% highlight ruby %}
# Gemfile
# ...

gem 'omniauth-facebook'
gem 'omniauth-google-oauth2'

# ...
{% endhighlight %}

OAuth requires that you send a user to a provider who then redirects back to
your site. This requires some setup on the provider's side beforehand. You can
go to the [Facebook developer area](https://developers.facebook.com/docs/facebook-login/)
to set up an app for Facebook and the [Google developer console](https://console.developers.google.com/)
to set up an app for Google. You will need two character strings for verification.
These are often called the "app ID" and "app secret". They are part of the
configuration you need in your devise initializer. "But wait!", you're saying
to yourself, how does the user model know about omniauth? The answer is that it
needs to be added to the `Alchemy::User` model. If you try to create an
`Alchemy::User` model in your Rails application it will clobber (replace) the
one from the `alchemy-devise` gem. A better approach is to add the devise
omniauthable functionality in an initializer. One place this can be done is in
the same place you are adding the credentials in the devise.rb initializer. In
the end the file will have this added to it:

{% highlight ruby %}
# config/initializers/devise.rb
# ...

Alchemy::User.class_exec {
  devise :omniauthable, :omniauth_providers => [:facebook, :google_oauth2]

  def self.from_omniauth(auth)
    where(provider: auth.provider, uid: auth.uid).first_or_create do |user|
      user.email = auth.info.email
      user.password = Devise.friendly_token[0,20]
    end
  end
}

config.omniauth :facebook, '<app_id>', '<app_secret>'
config.omniauth :google_oauth2, '<app_id>', '<app_secret>'

# ...
{% endhighlight %}

Not that a class method `.from_omniauth` is also being added. That will be used
in the next step.

Now that the user model has been modified to use omniauth and the configuration
for the providers is in place, you need to have code to handle the users who are
redirected back. This requires that you create omniauth callbacks. The code
looks like this:

{% highlight ruby %}
# app/controllers/alchemy/omniauth_callbacks_controller.rb
class Alchemy::OmniauthCallbacksController < Devise::OmniauthCallbacksController
  def facebook
    @user = Alchemy::User.from_omniauth(request.env["omniauth.auth"])

    sign_in_and_redirect @user, :event => :authentication
  end

  def google_oauth2
    @user = Alchemy::User.from_omniauth(request.env["omniauth.auth"])

    sign_in_and_redirect @user, :event => :authentication
  end
end
{% endhighlight %}

Note that each methd is named after its associated provider. The code in this
case is identical for each method and could be refactored into a single method.

Now that the callbacks can be handled, they need routes pointing to them. This
should be added to your routes file:

{% highlight ruby %}
# config/routes.rb
# ...

devise_for :user,
  class_name: 'Alchemy::User',
  controllers: {
    sessions: 'alchemy/user_sessions',
    :omniauth_callbacks => "alchemy/omniauth_callbacks"
  },
  skip: [:sessions, :passwords]

# ...
{% endhighlight %}

This is very close to the routes in the `alchemy-devise` gem.

Lastly, you need to provide a way for users to sign in. Here is an example
partial that can be included in site pages:

{% highlight erb %}
<%# app/views/shared/_sign_in.html.erb %>
<% if current_alchemy_user %>
  Signed in as <%= current_alchemy_user.email %> via <%= current_alchemy_user.provider.titleize %>
  <%= link_to "Sign Out", logout_path, method: :delete %>
<% else %>
  <%= link_to "Sign in with Facebook", user_omniauth_authorize_path(:facebook) %>
  <%= link_to "Sign in with Google", user_omniauth_authorize_path(:google_oauth2) %>
<% end %>
{% endhighlight %}

Overall, a convenient way to allow people to sign in to your Alchemy-based site.
