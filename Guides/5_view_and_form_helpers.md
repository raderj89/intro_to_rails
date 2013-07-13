#View and Form Helpers

Rails gives us access to some awesome, and super helpful, helpers to make our lives easier while we're putting together a page. These are (ordered by increasing complexity):

1. image_tag
2. link_to
3. form_for

Let's tage a look at each of these...

##image_tag

###Background

Instead of typing out image tags in HTML like this: ```<img src="…"/>```, Rails gives us a built in helper method ```image_tag``` to make our lives a bit easier.

###Example (Sinatra to Rails)

Before we get into the details of how to use the helper, here's a quick example. In Sinatra, we might say something like this:

    <img src="/images/deal_with_it.gif" alt="deal with it">

But in Rails, we can instead use the ```image_tag``` helper:

	<%= image_tag("deal_with_it.gif") %>
	
When the page loads, this code will get rendered as:

	<img alt="Deal_with_it" src="/assets/deal_with_it.gif" />

Here's another example, this time a bit more involved:

	image_tag("mouse.png", :size => "100x150", :mouseover => "/images/mouse_over.png")

When the page loads, this code will get rendered as:
    
	<img src="/images/mouse.png" onmouseover="this.src='/images/mouse_over.png'" onmouseout="this.src='/images/mouse.png'" width="100" height="150" alt="Mouse" />

Got it? Good. Now for a bit of explanation.

###The Basics
####Routes
Rails is really good about finding images (assuming they have unique names), so usually you don't need to append a path to the front of the image name unless you have nested folders within the asset pipeline. You saw that above when we simply passed in the image name to the ```image_tag``` helper. This will work for all images in your ```public``` directory and your ```assets/images``` directory.

By default, images that are stored in the assets folder will automatically be cached for future use as well. Therefore, it's best practice to put things like header images or splash pages in the assets folder.

Here's an example of an ```image_tag``` that uses a subdirectory within our normal ```assets/images``` path. In this case, we used a subdirectory of ```backgrounds```:

    <%= image_tag("backgrounds/binding_dark.png", :size => "200x200") %>

####Parameters

The ```image_tag``` helper takes any html image tag properties as comma seperated hash options. For example, you can specify ```alt``` or ```class``` parameters by way of key value pairs. Moreover, Rails gives you access to a couple really cool attributes:

* ```:size``` => "Height x Width"
* ```:mouseover``` => "Alternate_image_name.jpg"

The ```mousover``` attribute automatically creates the ```onmouseover``` and ```onmouseout``` attributes for you when the page renders, as seen in the example above.

---

##link_to

###Background

Instead of typing out html anchor tags like this: ```<a href="…">Link Here</a>```, we can use a built in helper method called  ```link_to``` to make our lives easier.

###Example (Sinatra to Rails)

Before we get into the details of how to use the helper, here's a quick example. In Sinatra, we might say something like this:

    <a class="users_link" href="/users/<%= @user.id %>">User Page</a>

But in Rails, we can say:

	<%= link_to "Google", "http://www.google.com" %>

A more realistic example might look something like this:

	<div class="navbar">
    	<%= link_to "Home", :root %>
    	<%= link_to "Sign Up!", :new_user, {:class => "sign_up"} %>
    </div>

When the page loads, that code will get rendered as:
    
	<div class="navbar">
	  <a href="/">Home</a>
	  <a href="/users/new" class="sign_up">Sign Up!</a>
	</div>

Got it? Good. Now for a bit of explanation.

###The Basics
####Prep Work

Using this helper to link to paths assumes that you have the following:

1. A route set up in your ```config/routes.rb``` file.
	* For example, using `resources :users`.
2. A controller set up to handle the route.

####Parameters

The basic format of the ```link_to``` tag is: 

	link_to( displayed_name, link_url, options )

1. The first parameter in the ```link_to``` tag is the body or display name. This is the link text that shows up on your web page.
2. The second parameter is the destination for the link. This can take the form of a full url (e.g.  ```http://google.com```), or a Rails path that you wish to utilize (e.g. ```:new_user``` or ```:tweets_path```).
3. The third parameter is an optional hash of ```URL Options``` and or ```HTML Options```.

Putting the three together, we get a link that looks like this:

    <%= link_to "JavaScript Docs", 
            "https://developer.mozilla.org/en-US/docs/Web/JavaScript",
            {:confirm => "Are you sure you want to learn about JavaScript? It sucks...", 
             :class   => "sucky-language",
             :id      => "javascript-sucks" } %>

This will get rendered on the page as:

	<a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript" class="sucky-language" data-confirm="Are you sure you want to learn about JavaScript? It sucks..." id="javascript-sucks">JavaScript Docs</a>
	
If you were paying attention, you might have noticed the ```confirm``` attribute in there. This will pop up a confirmation window in your browser when you click the link. If you click ``OK``, you'll be redirected. If you click `Delete`, you won't be. Fancy that!

---

##form_for

###Background

Ready to have your mind blown?

Remeber back in the old days (Friday), when we had to type out the nitty gritty details of each form, like the action and the method and the values and placeholders (or let's be real - copy and paste them)? Well, you can kiss that maddness goodbye. One of the super helpful features of Rails is a ```form_for``` method that does a lot of the leg work for you. Let's get started…

###Example (Sinatra to Rails)

As a reminder, this is how we used to do things in Sinatra:

	<form action="/users" method="post">
		<input type="text"     name="user[username]" placeholder="Username">
		<input type="email"    name="user[email]"    placeholder="email">
		<input type="password" name="user[password]" placeholder="Password">
		<input type="submit" value="Sign up!">
	</form>
 
But in Rails, this is how forms will look:

	<%= form_for @user do |f|  %>
	<%= 	f.text_field     :username, placeholder: "Username" %>
	<%= 	f.email_field    :email,    placeholder: "Email" %>
	<%= 	f.password_field :password, placeholder: "Password" %>
	<%= 	f.submit "Sign up!" %>
	<% end %>

Notice that this takes the form of a block (i.e. `do… end`). When this gets rendered, it looks like this:

	<form accept-charset="UTF-8" action="/users" class="new_user" id="new_user" method="post" >
	<div style="margin:0;padding:0;display:inline">
    	<input name="utf8" type="hidden" value="✓">
    	<input name="authenticity_token" type="hidden" value="fGUew7MnGx+48gel2vjxNhqdHVUKp7q7rIMCIk+ehOw=">
	</div>
	  <input id="user_username" name="user[username]" placeholder="Username" size="30" type="text" />
	  <input id="user_email" name="user[email]" placeholder="Email" size="30" type="email" />
	  <input id="user_password" name="user[password]" placeholder="Password" size="30" type="password" />
	  <input name="commit" type="submit" value="Sign up!" />
	</form>

You may be asking yourself, "WTF - why is there an extra div in my form?!?" Well, it's because Rails has our back.

> Now, you’ll notice that the HTML contains something extra: a div element with two hidden input elements inside. This div is important, because the form cannot be successfully submitted without it. The first input element with name utf8 enforces browsers to properly respect your form’s character encoding and is generated for all forms whether their actions are “GET” or “POST”. The second input element with name authenticity_token is a security feature of Rails called cross-site request forgery protection, and form helpers generate it for every non-GET form (provided that this security feature is enabled). You can read more about this in the Security Guide.

Here's another example with a simple search box, and the `form_tag` instead of `form_for`:

	<%= form_tag("/search", :method => "get") do %>
	  <%= label_tag(:q, "Search for:") %>
	  <%= text_field_tag(:q) %>
	  <%= submit_tag("Search") %>
	<% end %>

Which gets rendered as:

	<form accept-charset="UTF-8" action="/search" method="get">
	  <label for="q">Search for:</label>
	  <input id="q" name="q" type="text" />
	  <input name="commit" type="submit" value="Search" />
	</form>
	
####form_for vs form_tag

The `form_for` method takes as its first argument an activerecord object. This allows us to easily make a form to create or edit, or perhaps, destroy, that object. It also passes a form variable to the block, so that you don't have to repeat the model name within the form itself. It's the preferred way to write a model related form.

The `form_tag` method just creates a simple form tag, but still includes the Rails antiforgery hidden field. `form_tag` is best utilized in forms that don't deal with models, and is most commonly used in search forms. You'll notice a slight difference in how input fields in `form_tag` are written. For example, in `form_for` we use `f.text_field …`, while in `form_tag` we'd write `text_field_tag …`.

###The Basics
####Prep Work
Using a `form_for` method assumes the following things:

1. You have a model set up (e.g. `User`), which also implies that you have a migration set up.
2. You have an instance of the model available to you on the page (e.g. by way of `User.new` or ```User.find(params[:id])```)

####Walkthrough

Like our hard-coded HTML forms in Sinatra, `form_for` and `form_tag` have a standard set of fields such as text boxes, checkboxes, radio buttons, and submit buttons.

To begin, we start with a `form_for` declaration on our model of interest and add in our `do` block (this is very similar to how tables are created in activerecord).

	<%= form_for @user do |f|  %>
	<% end %>
		
Then, we list out the different fields we need, first telling it the type of field (e.g. `f.text_field`) and then the name of the field, and any additional field attributes we want to include.

	<%= 	f.text_field     :username, placeholder: "Username" %>
	<%= 	f.email_field    :email,    placeholder: "Email" %>
	<%= 	f.password_field :password, placeholder: "Password" %>
	<%= 	f.submit "Sign up!" %>

A few important things to note:

1. When this gets rendered, the form names get pushed into an array with the name of the variable you opened the block with. In this case, `user` was our variable, so the first field's value would be stored in `user[:username]`. As in Sinatra, the form attributes can be accessed in the `params` hash.
2. If the variable passed to `form_for` already has attributes that match the form fields, those fields will be populated with that variable's data. For example, a user's email would already be populated in the login form.
3. The default action is to make a post request to the base url of that object (e.g. `/users`).
4. The form automatically creates css id's for each input field. These can be customized using the `:namespace` option, as discussed below.

Catch all of that? Good, here's one more example:

	<%= form_for @person do |f| %>
	  First name: <%= f.text_field :first_name %>
	  Last name : <%= f.text_field :last_name %>
	  Biography : <%= text_area :person, :biography %>
	  Admin?    : <%= check_box_tag "person[admin]", @person.company.admin? %>
	  <%= f.submit %>
	<% end %>

One last thing. The `form_for` takes a few optional paramters passed in thusly: 

	form_for @post, :as => :post, :url => post_path(@post), :method => :put, :html => { :class => "edit_post", :id => "edit_post_45" }

* `:url` - This, obviously, is the url we want to submit this form to. It can also be a route as well as a hard-coded url.
* `:namespace` - This, guarantees unique id attributes for all form elements. The namespace attribute gets prefixed and underscored onto each generated HTML id.
* `:html` - Optional HTML attributes for the form tag.

---

And that's it for view and form helpers for now! There's a ton of other ones, so be sure to check the docs when you find yourself scratching your head.
