# Part 3: Create CRUD routes, actions, and views for Movies

Start the app with `rails server`.
Once the server has started, GitPod will show a small popup box asking if a port should be exposed, then saying that service is running. Click on `Open in Browser`. Alternatively, click on the ports at the bottom of the GitPod IDE to access the ports tab.

The URL that will open in the browser will be something like `https://_workspace_id.ws-eu01.gitpod.io/`. We will
refer to this as the app's _root-URI_. A URI
such as this one that specifies only a hostname (and possibly a port)
will fetch the app's home page.  For a brand-new Rails app without any
code, this page is the  "Welcome to Rails" splash page you see.  

## Create Routes
If you now try the URI _root-URI_/`/movies`
(that is, 
`https://`_workspace_--id.`gitpod.io/movies`
if developing on GitPod, you
should get a Routing Error from Rails.  Indeed, you should verify that
_anything_ you add after the hostname part of the URI results in this error, 
because we haven't specified any routes
 mapping URIs to app
methods.  Try `bundle exec rake routes` and verify that 
it informs us that there are no routes in our brand-new app.  
(You may want to open multiple Terminal windows or tabs so that the app can keep
running while you try other commands.)

More importantly,
use an editor to open the file `log/development.log` and observe that
the error message is logged there; this is where you look to find
detailed error information when something goes wrong.  We'll show other
problem-finding and debugging techniques later.

To fix this error we need to add some routes.  Since our initial goal is
to store movie information in a database, we can take advantage of a
Rails shortcut that creates RESTful
routes for the four basic CRUD
actions
(Create, Read, Update, Delete) on a model.  (Recall that
RESTful routes specify self-contained requests of what operation to
perform and what entity, or resource, to perform it on.)  Edit
`config/routes.rb`, which was auto-generated by the
`rails new` command and is heavily commented.  Replace the contents of
the file with [this
code](https://gist.github.com/armandofox/294ff740da2b016784c8)
(the file is mostly comments, so you're not
actually deleting much).

Save the `routes.rb` file and run `rake routes` again, and observe
that because of our change to `routes.rb`, the first line of output
says that the URI `GET /movies` will try to call the `index` action of
the `movies` controller; this and most of the other routes in the
table are the result of the line `resources :movies`, as we'll soon
see.  (As with many Rails methods, `resources 'movies'` would also
work, but a symbol usually indicates one of a fixed set of choices
rather than an arbitrary string.)  The root route `'/'`,
RottenPotatoes' "home page," will take us to the main Movie listings
page by a mechanism we'll soon see called a URL redirection.

(If you want more practice with how the `routes.rb` contents get
parsed into routes, play around with the [Rails Routing Practice
app](https://rails-routing-practice.herokuapp.com) brought to you by
ESaaS.)


Using convention over configuration, 
Rails will expect
this controller's actions to be defined in the class
`MoviesController`,
and if that class isn't defined at application
start time, Rails will try to load it from the file
`app/controllers/movies_controller.rb`.  Sure enough,
if you now reload the page  _root-URI_`/movies` in your
browser, you should see a different error: `uninitialized constant
  MoviesController`.  This is good news: Rails is essentially
complaining that it can't find the `MoviesController` class, but the
fact that it's even looking for that class tells us
that our route is working correctly!  As before, this error
message and additional information are captured in the log file
`log/development.log`. 

## Create Controller
- Create a controller to handle the RESTful routes we've setup. Create a new file `app/controllers/movies_controller.rb`, and add the following contents:
```
class MoviesController < ApplicationController
  def index
    @movies = Movie.all
  end
end
```

## Create View
- Create a new view template file `app/views/movies/index.html.haml`, and add the following haml code:
```
-#  This file is app/views/movies/index.html.haml
%h1 All Movies

%table#movies
  %thead
    %tr
      %th Movie Title
      %th Rating
      %th Release Date
      %th More Info
  %tbody
    - @movies.each do |movie|
      %tr
        %td= movie.title 
        %td= movie.rating
        %td= movie.release_date
        %td= link_to "More about #{movie.title}", movie_path(movie)
```

## Summary

You've used the following commands to set up a new Rails app:

  0. `rails new` sets up the new app; the `rails` command also
    has subcommands to run 
    the app locally (`rails server`) and other management tasks.
  0. Rails and the other gems your app depends on (we added the Haml
    templating)
    are listed in the app's `Gemfile`, which Bundler uses to automate
    the process of creating a consistent environment for your app
    whether in development or production mode.
  0. To add routes
   in 
    `config/routes.rb`, the one-line `resources` method provided by
    the Rails routing system allowed us to set up a group of related
    routes for CRUD
	index{Representational State Transfer (REST)!CRUD|textit}% 
    actions on a RESTful resource.
  0. The log files in the `log` directory collect error information
    when something goes wrong.

You may have noticed that after changing `routes.rb`, you didn't
have to stop and restart the app in order for the changes to take
effect.  In development mode, Rails reloads all of the app's classes
on every new request, so that your changes take effect immediately.
In production this would cause serious performance
problems, so Rails provides ways to change various app behaviors
between development and production mode.


<details>
<summary>
  Recall the generic Rails welcome page you saw when you first created
  the app.
	In the <code>development.log</code> file, 
  what is happening when the line <code>Started GET
	"assets/rails.png"</code> is printed?  (Hint: recall the steps needed to
  render a page containing embedded assets.)

</summary>
<blockquote>
</blockquote>
    The browser is requesting the embedded image of the Rails logo for the
    welcome page.
</details>

### Next Step
[Part 5: Deploy to the cloud, including production database](Part5.md)
