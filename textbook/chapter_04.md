Chapter 4 Walkthrough

Note:  you'll find it very useful to have two terminals open at a time:  one in which to issue commands, and the other in which to keep rails running.

Proceed as directed until 

> Edit your Gemfile by adding the following (anywhere in the file, as long as itís not inside a block beginning with group).

Fox, Armando; Patterson, David (2014-01-03). Engineering Software as a Service: An Agile Approach Using Cloud Computing (Kindle Locations 3266-3268). Strawberry Canyon LLC. Kindle Edition. 

You can ignore adding the debugger gem since it is no longer used, and the byebug gem is already present.  However, you will have to add the haml gem. 

The next place to update the instructions is

Start the app with rails † server and point a browser to http:// localhost: 3000.

Fox, Armando; Patterson, David (2014-01-03). Engineering Software as a Service: An Agile Approach Using Cloud Computing (Kindle Locations 3291-3292). Strawberry Canyon LLC. Kindle Edition. 

If you're using cloud9, you must issue thia command instead, and then navigate to the link generated within c9:

```
rails s -p $PORT -b $IP
```

View the welcome page, then append  /movies  to your current URL to get a Routing Error, instead of

If you now visit http:// localhost: 3000/ movies, you should get a Routing Error from Rails.

Fox, Armando; Patterson, David (2014-01-03). Engineering Software as a Service: An Agile Approach Using Cloud Computing (Kindle Locations 3294-3295). Strawberry Canyon LLC. Kindle Edition. 

When you're asked to replace the contents of the config/routes.rb file, ignore lines 1 and 4 (since they already exist, and line 1 given in Pastebin has been changed in Rails4), and just sandwich lines 2 and 3 into the file and save it.  Now if you run rake routes, you should see the RESTful routes associated with the movies resource

Note the Elaboration section explaining one of the major differences between running our app in development vs. running it in production: 
 
ELABORATION: Automatically reloading the app

Fox, Armando; Patterson, David (2014-01-03). Engineering Software as a Service: An Agile Approach Using Cloud Computing (Kindle Location 3332). Strawberry Canyon LLC. Kindle Edition. 

The next part that should be updated is:

```ruby
Create a file app/ models/ movie.rb containing just these three lines: http:// pastebin.com/ 1zatve2r 
1  class † Movie † < † ActiveRecord:: Base 
2 † † † attr_accessible †: title, †: rating, †: description, †: release_date
3  end
```

Fox, Armando; Patterson, David (2014-01-03). Engineering Software as a Service: An Agile Approach Using Cloud Computing (Kindle Locations 3439-3443). Strawberry Canyon LLC. Kindle Edition. 

Instead of handling mass assignment w/ att_accessible in our model, we will be adding code to our controller:  your file should only contain lines 1 and 3 (skip line 2)

You can go ahead and seed the database as shown, but when you create your controller, you will have to add this additional code to app/controllers/movies_controller.rb (in addition to the code that has been provided) in order to safely permit mass_assignment:

```ruby
# add below all other methods
private

def movie_params
    params.require(:movie).permit(:title, :rating, :description, :release_date)
  end
```
You will then proceed to add controller, view, and stylesheet code as directed.

When you get to the section on debugging, be aware that Rails4 uses byebug for debugging, so that you no longer have to start up the server with --debugger, and that you can insert debugger OR byebug at the point in your code where you want to stop the server (use -debugger and/or -byebug within your haml code)

The second way to debug correctness problems is with an interactive debugger. We already installed the debugger gem via our Gemfile; to use the debugger in a Rails app, start the app server using rails server --debugger, and insert the statement debugger at the point in your code where you want to stop the program. When you hit that statement, the terminal window where you started the server will give you a debugger prompt. In Section † 4.7, weíll show how to use the debugger to shed some light on Rails internals.

Fox, Armando; Patterson, David (2014-01-03). Engineering Software as a Service: An Agile Approach Using Cloud Computing (Kindle Locations 3826-3830). Strawberry Canyon LLC. Kindle Edition. 

In Section 4.7 you can ignore the discussion about mass assignment (since it's handled differently in Rails4), ignoree directive to comment out a line of code in config/environments/development.rb, and modify your create method code as follows (instead of what is provided):

```ruby
  def create
    #@movie = Movie.create!(params[:movie]) #old way
    @movie = Movie.create!(movie_params)  # new way
    flash[:notice] = "#{@movie.title} was successfully created."
    redirect_to movies_path
  end
```

Note that this code now calls your private movie_params method that you added above.

When you get to the part where you add the edit and update methods to the controller, you'll have to make the same change to the update method:

```ruby
  def update
    @movie = Movie.find params[:id]
    #@movie.update_attributes!(params[:movie])  # old way
    @movie.update_attributes!(movie_params)  # new way  
    flash[:notice] = "#{@movie.title} was successfully updated."
    redirect_to movie_path(@movie)
  end
```

The remainder of the walkthrough (adding the destroy method and adding a delete link to the show page) can be followed as is.





