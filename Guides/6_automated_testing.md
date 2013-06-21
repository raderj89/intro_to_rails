#Automated Testing

I purposefully left testing to the end, even though you will here the term 'test driven development' (TDD) a whole lot in the coming weeks.

	Test Driven Development is a software development process that is focuses on the developer writing failing tests for the functionality they wish to have in their app, then writing code that will cause the tests to pass, providing the desired functionality, and finally, refactoring the code to make it faster. -Wikipedia

Basically, as apps get bigger and bigger, bugs become more and more prevalent. One of the ways to make reliable code is to generate tests that confirm its reliability. As long as you run those tests every time you make a change (and, in the case of TDD, make changes to get the tests to pass), you know that you're good to go. It's a great way to ensure quality and find bugs as soon as the appear, and can be a really powerful tool in the right hands.

Unfortunately, it's also yet another language to learn and can really hinder your ability to pick up Rails if given too much weight early on. So, I this guide will serve as an introduction only; the real learning will happen once you've gained a semblance of comfort with Rails itself.

---

One thing to note right off the bat is that Rails comes with three basic environments: `development`, `test`, and `production`. They mean what you'd expect, so I won't beat a dead horse there (is that actually a saying?). For our purposes, we'll now be using the `test` environment.

In your `Gemfile`, add this block to the bottom to include rspec. This requires the rspec gem for our `development` and `test` environments only.
	
	group :development, :test do
		gem 'rspec-rails'
	end

After bundling, we can use rspec's built in generator to create a new spec:
 
	rails generate rspec:install

This command will
	
1. create a `spec` in the root of our application in which we can store and organize our test files, or `specs`, and

2. create other files that you don't need to worry about just yet.
    
Now, every time you generate a model using the Rails generate command, Rails will automatically generate a spec file for this model and place it in the `spec/models` directory. Can you say awesome? Or, overbearing?

Once you put rspec tests in each file, you will be able to run your tests using `rspec spec` (runs all commands) or `rspec spec/directory/file` to run just one file's tests.

Here is an example of what a test file might look like:

	require 'spec_helper'
	
	describe 'User' do
		
	  context 'create' do

	    it 'should create a new user' do
	      expect { user.create(:name => "Mitch", :password => "secret") }.to change(User, :count).by(1)
	    end

	    it 'should require a name >= 4 characters' do
	      user.create(:name => "No", :password => "secret").should_not be_valid
    	end
    	
    	it 'should verify the uniqueness of the name'
    	
      end
    end
    
One nice thing about rspec is that it almost reads like english. Here's the same code, this time with comments to explain what's going on:

	# requires the helper file
	require 'spec_helper'
	
	# these tests are in relation to the user model
	describe 'User' do
		
	  # this is a way to make subgroups within the 'User' tests
	  context 'create' do

		# this is the messsage that will display when the test runs/finishes
	    it 'should create a new user' do
	      # this is the meat of the test, which checks to make sure we can create a new user
	      expect { user.create(:name => "Mitch", :password => "secret") }.to change(User, :count).by(1)
	    end

	    it 'should require a name >= 4 characters' do
	      # this is more rspec magic that checks to make sure we can't create an invalid user
	      user.create(:name => "No", :password => "secret").should_not be_valid
    	end
    	
    	# this is a pending test which won't throw an error, but let's us know we have more to do
    	it 'should verify the uniqueness of the name'
    	
      end
    end

And that's all the introduction I'm going to give you for now. You'll become very familiar with rspec testing soon enough, but for now just be aware that it exists and that its important is proportional to the complexity of your app.
