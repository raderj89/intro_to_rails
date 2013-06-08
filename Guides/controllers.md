#Controllers

Controllers are on the receiving end of the routes file that you just read about. Basically, a controller is a Ruby class that responds to requests, manipulates model(s) as necessary and tells a particular page to display. 

Here is what a basic controller looked like in Sinatra:

	get '/' do
	  @user = User.new
	  erb :index
	end

	get '/users/:id' do
	  @user = User.find(params[:id])
	  erb :profile
	end

But in Rails, our controller now looks like this:

	class UsersController < ApplicationController

	  def index
	    @user = User.new
	  end

	  def show
	    @user = User.find(params[:id])
	  end
	  
	end

There's some trickery going on here. Besides the fact that the format is different (we're now using explicit method definitions to in our controller), we also don't have to call `erb :something`; Rails does this automatically! If you name your files correctly, Rails will automatically load the proper view at the end of the method, unless you override the default behavior by explicitly telling it to do something else. Sahweeet!

Another thing you'll notice is that `UsersController` inherits from `ApplicationController`. If you were taking notes while you were reviewing the skeleton Rails app, that name may seem familiar. In fact, the `ApplicationController` is already defined for you and, by convention, every other controller should inherit from that. That way, you can put useful methods in the `ApplicationController` and they'll be accessible to all of your other controller. For example:

	class ApplicationController < ActionController::Base
		def methods_every_controller_can_use
		end
	end

	class UserController < ApplicationController
		def method_specific_to_user_controller
		end
	end

Again, the first class you see above is standard to every new Rails app.

So, when your application receives a request the routes file will determine which controller and action to run. Rails then creates an instance of that controller and runs the method with the same name as the action.

For example, an incoming route of `/clients/new` would look for something like:

	class ClientsController
	  def new
	  end
	end

Which in turn will run anything contained within the `new` action and then display the default view of `/views/clients/new.html.erb`, but we'll get to that a bit later.

Other than that, the basic functionality of a controller doesn't change much from what you did in Sinatra.

##Filters

Filters are methods to be run before, during or after other controller actions. They are especially handy for running validations before (or after) visiting a route.

	class ApplicationController < ActionController::Base
	  before_filter :require_login 
	
	  private

	  def require_login
	    unless logged_in?
	      flash[:error] = "You must be logged in to access this section"
	      redirect_to new_login_url # halts request cycle
	    end
	  end
	end

What you see above will allow you to check that a user is logged in before visiting any link on your site. Sweet, but a little overkill!

Obviously you don't necessarily want this to run before ALL actions, so you can skip filters like this:

	class UsersController < ApplicationController
	  skip_before_filter :require_login, :only => [:new, :create]
	end

This will skip the `before_filter` for the `#new` and `#create` actions (obviosuly, you don't want to force your users to be logged in to create a new account).

After filters do the same exact thing but *after* the controller action is called.


	class ApplicationController < ActionController::Base
	  after_filter :stuff_to_do_after_action
	end

Finally, around filters are general purpose filters that can run code before **and** after the action. The use of the `yield` keyword determines exactly when the action is performed.


	class ApplicationController < ActionController::Base
		around_filter :stuff_before_and_after_action
	
		def stuff_before_and_after_action
			stuff_before
			yield # this is the action itself
			stuff_after
		end
	end
	
Don't worry to much about learning this all right now, but do be aware the it exists and can really make your life easier.