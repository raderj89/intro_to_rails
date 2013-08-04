#Routes

Routes are probably the most confusing part of learning how to put together a Rails app, so brace yourself. Again, the routes file can be found at `config/routes.rb` and is very, very important. Basically, the file exists to tell Rails how to direct any incoming `get` requests from a user as well as `post`, `put` and `delete` requests that, for example, a form may try to access. It's almost like a switchboard of sorts. You visit the url `www.yoursuperawesomewebsite.com/home` and the routes file decides where to send you. Likewise, a form may try to send a `post` request to `users/new` and the routes file tells you where to go.

Enough with the talk. Here's an example of a slightly simplified routes file:

	BrahooAnswers::Application.routes.draw do
	  # Questions
	  resources :questions

	  # Answers
	  resources :answers,  :only => [:create, :edit, :destroy]

	  # Comments
	  resources :comments, :only => [:create, :edit, :destroy]

	  # Users
	  resources :users
	  match '/signup' => 'users#new'

	  # Sessions
	  get    '/login'  => 'sessions#new'
	  post   '/login'  => 'sessions#create'
	  delete '/logout' => 'sessions#destroy', :via => :delete

	  # Root
	  root :to => 'questions#index'
	end

There's a lot of new stuff going on here. I would love to tell you that it will all make sense in a day or two, but that's probably not realistic.

No use in sidestepping though, let's jump right in. The first thing you'll notice is this `resources` word being tossed around. That is some Rails magic that creates a bunch of routes for you. At any time in your app, you can hop over to the command line and type `rake routes` to see a list of all of the routes that you have available. Get used to that command, because you'll use it often! By you saying `resources :users`, you're adding **seven** new routes to your app. Here's a brief overview of what you get:


    HTTP Verb |Path             |Controller & Action|Use case
    ----------|-----------------|-------------------|----------------------------
    GET       |/users           |=> user#index      |display a list of all users
    GET       |/users/new       |=> user#new        |return an HTML form for creating a new user
    POST      |/users           |=> user#create     |create a new user
    GET       |/users/:id       |=> user#show       |display a specific user
    GET       |/users/:id/edit  |=> user#edit       |return an HTML form for editing a user
    PUT       |/users/:id       |=> user#update     |update a specific user
    DELETE    |/users/:id       |=> user#destroy    |delete a specific user

Rather than reinvent the wheel, I'm going to outsource the rest of your learning on this part. Keith Tom has put together an excellent guide that I strongly encourage you to read. In fact, if you only have time for one guide, stop reading this one and go through his. You can find it <a href="https://gist.github.com/keithtom/3f311c392326bc659b54#readme" target="_blank">here</a>.

Now that you've done that, you can proceed on to the controller section of this guide to get a little bit more context on the rest of the picture.

If you are somehow still hungry for more information on this whole routes thing before you move on, I've copied the routes file from above and commented a few of the more interesting parts to whet your appetite. This is purely extra credit, so feel free to move on now if you'd like.


    BrahooAnswers::Application.routes.draw do
      # Questions
      ## automatically makes seven routes for us
      resources :questions

      # Answers
      ## we can limit which routes are created using :only
      resources :answers,  :only => [:create, :edit, :destroy]

      # Comments
      resources :comments, :only => [:create, :edit, :destroy]

      # Users
      resources :users
      ## define a custom url and tell it where to go
      ## in this case, the new method in the users controller
      match '/signup' => 'users#new'

      # Sessions
      ## :as gives us the ability to access this url in forms or links
      ## by saying login_path or login_url as opposed to explicitly
      ## typing out the url
      get    '/login'  => 'sessions#new', :as => :login
      post   '/login'  => 'sessions#create'
      delete '/logout' => 'sessions#destroy', :via => :delete

      # Root
      ## specificies what gets called when you visit '/'
      root :to => 'questions#index'
    end
    
That's it for Routes, so jump on over to the [Controllers](3_controllers.md) page.
