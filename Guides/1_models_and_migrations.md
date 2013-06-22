#Models and Migrations

Models and migrations are models and migrations, meaning that if you learned them in Sinatra there's nothing new in Rails. The one major difference is in generating the two automatically.

Creating a model in our sinatra skeleton went like this:

	$ rake generate:model NAME=User

This was actually a custom rake task written by Dev Bootcamp. In Rails, this rake task already exists! If you don't believe me, type `rake -T` to see a list of all of the rake tasks available to you out of the box (there are a lot). So, to make a new model in Rails, simply type:

	$ rails generate model User

This will create a model in `/assets/models` named `user.rb` and a matching migration named `create_users.rb`. If you want to get extra fancy, you can actually pass in table attributes directly from the command line. For example:

	rails generate model User username:string password:string

This would create a table that looks like:

	class CreateUsers < ActiveRecord::Migration
	  def change
	    create_table :users do |t|
	      t.string :username
	      t.string :password

	      t.timestamps
	    end
	  end
	end

Say whaaaat?! Pretty awesome, right?

As for the rest of the models and migrations, not much else changes. ActiveRecord is still ActiveRecord, so your model and table validations (e.g. `:uniqueness => true`) don't change.

Onwards and upwards!
