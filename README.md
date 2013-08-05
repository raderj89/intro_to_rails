#An Intro to Rails 3
-By some newbies

===

If you're reading this file, you're most likely trying to learn Rails. That's great! You're about to have a whole lot of new power at your fingertips.

Unfortunately, as with any new language or framework, there's a bit of a learning curve that goes along with Rails. You'll need to spend some time getting familiar with its intricacies, peculiarities, and best practices. And yes, 'some time' is an understatement.

For a lot of people this can be an especially difficult process. And that's why I decided to put this guide together.

First things first, some background:

I'm a newly-learned (pronounced learn-ed) Dev Bootcamp student fresh off my first week of Rails. This guide largely draws from the work of my classmates (with the notable addition of my spelling mistakes and poor grammar) and is intended to make the on boarding process as easy and pain free as possible for those following behind us.

Note: You will see many references to how things were done in Sinatra. If you're not a Dev Bootcamp student and have never heard of or used Sinatra before, feel free to glance over those parts.

Okay, enough with the talk. Onward!

---

##Setup

To get started with Rails, make sure you have it installed on your computer. You can type `which rails` or `gem list` to see if it is already installed. If not, just type `gem install -v=3.2.13 rails` to download it. (If you only type 'gem install rails', you'll get Rails 4.0 since it's the latest version).

Once you have that, navigate to a new directory of your choosing.

In Sinatra, you were probably given a skeleton app to use and you would have to oh-so-laboriously copy and paste it into the new directory. In Rails, simply type `rails new name_of_app_here` in the terminal to setup your skeleton automagically. For our purposes, let's add a `-T` flag at the end of that statement to tell Rails that we want to use rspec and not the default test unit platform (`-T` is short for `--skip-test-unit`). So it'll look like this:

    $ rails new name_of_app_here -T

Or, if you have multiple versions of Rails installed:    

    $ rails _3.2.13_ new name_of_app_here -T

You should see a metric ton of `create` and `Using` statements appear on the screen when you hit enter. Now, if you look at your directory you'll see a folder with your app in it. If you navigate into it, you'll a whole bunch of new and confusing files. Don't panic just yet! We'll cover the important ones soon enough.

---

Speaking of important ones, let's dig into that directory a bit. Rails is very much structured in the Model - View - Controller way. Here is a (very) brief overview of the more important parts that a *beginner* should become familiar with:

`APP`

* `assets`:

	In Sinatra, you were probably used to putting your CSS and JavaScript files in your public folder. Now, they're stored in this assets folder (or pipeline, as you'll start to call it).

* `controllers`:

	You'll never guess which part of the Model - View - Controller structure is stored in this directory. This houses the conductors of your application.

* `helpers`:

	Helpers will be useful later on, but for now just know that this is the place to store your cross-app helper methods and the like (for example, `current_user`).

* `models`:

	The place where all of your models go. Duh.

* `views`:

	The last of the MVC trifecta. This is where you store nearly every page that your app will display.

`DB`

* `migrate`:

	If you're coming off of Sinatra, this should look exactly the same to you. All of your table modifications - create, update or otherwise - will take the form of migrations stored in this directory.

Outside of the APP and DB directories, there are two other very important files:

* `Gemfile`

	Rails is awesome and automagically requires all of the files in your Gemfile. Get familiar with the gems that come pre-included, or at the very least know what they're related to. If you see `group :test` or similarly worded statements, that means the gems will just be required for that specific context (in this case, when you're conducting testing using rspec).

* `config/routes.rb`

	The routes file will be completely new to most of you, but it's importance can't be understated. This file gives you control over how to route all sorts of requests (```get '/'```, ```post '/users/new'```, etc.) and will be the topic of a deep dive later on in this guide.

If you navigate into any of these files, you'll notice that a lot of them already have detailed comments describing their purpose. Skim through those! By no means do you need to understand all of them, but it can't hurt to get a general sense of what's going on.

---

##Guides

Okay, so now that you are at least vaguely familiar with the layout of a Rails app, we're going to deep dive into a few topics that will help get you moving along to Rails mastery. Those topics are:

1. [Models and Migrations](/Guides/1_models_and_migrations.md) 
2. [Routes](/Guides/2_routes.md) 
3. [Controllers](/Guides/3_controllers.md) 
4. Views and Partials
5. [View and Form Helpers](/Guides/5_view_and_form_helpers.md) 
6. [Automated Testing](/Guides/6_automated_testing.md) 
7. [jQuery and Ajax](/Guides/7_jquery_and_ajax.md) 

Each of these topics are covered in a separate markdown file. I'd recommend covering them in the order listed, but it's certainly not a requirement. By the end of this guide, you'll have a good sense of how a Rails app works and will be able to build a functioning app of your own.

What are you waiting for? Go get started!
