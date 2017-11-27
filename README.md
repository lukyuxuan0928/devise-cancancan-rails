# Devise & CanCanCan

## Devise

Devise is a flexible authentication solution for Rails based on Warden.
It's composed of 10 modules:

  - Database Authenticatable
  - Omniauthable
  - Confirmable
  - Recoverable
  - Registerable
  - Rememberable
  - Trackable
  - Timeoutable
  - Validatable
  - Lockable

## CanCanCan

CanCanCan is an authorization library for Rails.

## Getting Started

Here I'm using a simple application to apply the features of Devise & CanCanCan on rails.

## Installation

Before build up the application, we need to do some installation

### Devise

Before using the devise authentication features, you need to add it in GemFile:
```
    gem 'devise'
```

Then, you need to install the devise:
```
    $ rails generate devise:install
```

Configue the mailer options in 'config/environments/development.rb':
```
    config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
```

Generate the model with specific name (default: devise):
```
    $ rails generate devise User
```

Run database migration:
```
    $ rake db:migrate
```

### Example

Some of the example that showing how to use the features which is not default by devise.

#### Extra attributes

To do additional attributes on sign up or update account:

```
  class ApplicationController < ActionController::Base
    before_action :configure_permitted_parameters, if: :devise_controller?

    protected

    def configure_permitted_parameters
      devise_parameter_sanitizer.permit(:sign_up, keys: [:username])
      devise_parameter_sanitizer.permit(:account_update, keys: [:name])
    end
  end
```

#### Trackable

In orde to use the features of trackable, you need to uncomment the migration file:

```
    ## Trackable
    t.integer  :sign_in_count, default: 0, null: false
    t.datetime :current_sign_in_at
    t.datetime :last_sign_in_at
    t.inet     :current_sign_in_ip
    t.inet     :last_sign_in_ip
```

#### Confirmable

In order to use the features of lockable, you need to add it in 'devise.rb' model file:

```
    :lockable
```

Uncomment the migration file:

```
    ## Confirmable
    t.string   :confirmation_token
    t.datetime :confirmed_at
    t.datetime :confirmation_sent_at
    t.string   :unconfirmed_email # Only if using reconfirmable
```

#### Lockable

In order to use the features of lockable, you need to add it in 'devise.rb' model file:

```
    :lockable
```

After that, uncomment the required setting in 'initializers/devise.rb':

```
    config.lock_strategy = :failed_attempts
    config.unlock_keys = [:email]
    config.unlock_strategy = :email
    config.maximum_attempts = 2
```

Uncomment the migration file:

```
    ## Lockable
    t.integer  :failed_attempts, default: 0, null: false # Only if lock strategy is :failed_attempts
    t.string   :unlock_token # Only if unlock strategy is :email or :both
    t.datetime :locked_at
```

### Timeoutable

In order to use the features of timeoutable, you need to uncomment the setting in 'initializers/devise.rb':

```
    config.timeout_in = 30.minutes
```

### CanCanCan

Before using the features of CanCanCan, we need to install the gem:

```
    gem 'cancancan', '~> 2.0'
```

Install the ability:

```
    $ rails g cancan:ability
```

Add it into top of controller:

```
    load_and_authorize_resource
```

### Example

Some of the example that using cancancan:

Identify authority by user:

```
    user ||= User.new # guest user (not logged in)
    if user.admin?
      can :manage, :all
    else
      can :manage, Post, user_id: user.id
      can :read, :all
    end
```

Control the display
```
  <% if can? :update, post %>
    <td><%= link_to 'Edit', edit_post_path(post), :class => 'btn btn-default' %></td>
  <% end %>
```


## Version

Please take note that might minor changes of syntax on different version

```
    ruby              ==   2.4.1
    rails             ==   5.1.4
    rvm               ==   1.29.3
    curl              ==   7.47
    nodejs            ==   4.2.6
    clearance         ==   1.15.1
    jquery-rails      ==   4.31
    bootstrap         ==   4.0.0.beta2.1
    devise            ==   4.3.0
    pg                ==   0.21.0
    cancancan         ==   2.1.2
```
