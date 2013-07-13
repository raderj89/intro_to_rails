#jQuery and Ajax

Rails has some built in helpers that do some fancy Ajax stuff for you. **DON'T USE THEM**. One day down the line, you may know enough to start using them. Until you reach that state of mastery, avoid the built in helpers.

For all intents and purposes, Ajax and jQuery are the exact same in Rails as they were in Sinatra. You interact with the DOM the same way, you make post and get requests the same way, you animate the same, etc.

Okay, so maybe not *exactly* the same. One nice thing about Rails is that you can use a nifty new trick in the controllers to route Ajax requests. Here's what that looks like:

	respond_to do |format|
      format.html { redirect_to :back }
      format.json { render :json => {count: count} }
    end

In Sinatra, some of you may have used the `response.xhr?` call to respond differently to JavaScript requests. You still have the ability to do that, but now you can also use the `respond_to` block to control how you respond to requests for different data types.

For example, say you make a `post` request from JavaScript and tell it you're expecting `json` data back. It would hit this `respond_to` block, go to the `format.json` line because JavaScript wants `json` back, and return the value executed within that call, in this case a `json` object with a `count` variable in it.

Keeping with the theme of this guide, don't worry about knowing exactly how to use this in your code. For now, know that this is yet another tool that you can keep in your back pocket for when the time comes.
