
# Structuring our app

## About the web application


Today we are going to build a little web application, but first we have to think about exactly what that means - what do we need to do in order to make that happen?

There are a few basic elements.

Firstly, we need to have a page in an internet browser that will allow users to type information into forms, just as if they are writing an email in GMail or Yahoo! Mail. The pages need to be sent to the browser as HTML. Users need to be able to click on a button that will make the web browser send the data they have typed off to the application, which is on a remote server somewhere in the world waiting to be told what to do.

Then the application needs to work out what to do with the information. Maybe you are adding new data, maybe you are deleting data that you no longer need, and maybe you are changing data.

The data also needs to be saved somewhere, by something that will not only remember it, but will also allow it to be found again.

Lastly, we need to be able to see web pages that show the information that we have previously stored.

There are quite a few different things that have to be done in there, and the Ruby on Rails framework provides different components that specialise in doing different tasks.

* “Views”, which create the HTML pages that are sent to the browser.
* “Controllers”, which work out what to do with information sent from the browser, and which views then need to be used to send pages back again.
* A database, for storing and retrieving information.
* “Models”, which tell the Ruby on Rails system how to store and modify data in the database.

##About the design

We are going to build a little system for managing publisher metadata. So what do we need to store information about?

The world is full of “stuff”. Books, cats, football matches, emails, office buildings, submarines, string quartets, rock concerts. Some of them are actual physical things, some of them are abstract, but all of them can be thought of as some kind of object.

Each of the above examples of stuff has a bunch of information associated with it.

* What time will the performance begin? “8:30”
* How deep can the submarine go? “1,000 ft”
* What’s the name of your cat? “Rosemary”
* Who did you send the email to? “The prime minister”
* What’s the ISBN of that book? “978-0205309023”

These are all “attributes” of the object.

Each of the above “stuffs” has things that you can do to them.

* The performance can be cancelled.
* The submarine can be painted
* The cat’s name can be changed to “Ambrose”
* The email can be sent
* The book can be bought.

If we’re going to build a publishing management system, what sort of objects do we need?

Publishers make books, so we'll need to describe them.  We'll try to use words that are as precise as possible, so if any other developers come to work on our code, it'll be easy for them to figure out what our app does.

The first model we'll create will be called "Work", and it will have a title, a description, a subject and a cover image.

The second model will be called "Product". We could call it "book", but that wouldn't be very helpful if we want to store information about apps, or maps, or marketing materials, or other non-book products, in our system. The word "book" can also mean "individual copy" which would be misleading. Our products will have a format, a price, an ISBN, a publication date and a page count.

Let's say that we have two products. The first is a paperback, published in June 2015. The second is a hardback, published in March 2015.

Both these products will belong to a work, whose title is "War and Peace Revisited".  The work's subject is "fiction". The title and the subject from the work will be inherited by the products. We don't need to repeat them.  

\begin{aside}
\label{aside:normalisation}
\heading{Principle: Normalisation}
\noindent

We don't store the same data more than once in a database.


\end{aside}


## Create our first model

We'll keep the server running so we can see our changes as they happen. So let's open a new console window. At the bottom of the Nitrous page, click the + sign next to Console. Type the following to switch to your app's folder:

```
  cd code/book_organiser
```

Now we can run commands which our Rails app will understand. Type the following:

```
  rails generate scaffold work name:string description:text cover:string subject:string
```

This has created a new file with some code in called a migration. When we run this migration code, it'll tell the database to set up a new table, with a column called "name", a column called "description", a column called "cover" and a final column called "subject".

The database will expect most of the columns to contain data that is a 'string': short pieces of text up to 256 characters. The description column, however, will contain pieces of text with no length limit. Telling the database what sort of data it should be expecting makes for good quality data. For example, if the database was expecting a date, but a user tried to put in a string -- say, "Monday", or "Next week" -- the database wouldn't let the user save the data and would insist on an actual date.


Type the following to run the migration code:  

```
  rake db:migrate
```

Rake is a tool you can use with Ruby projects to run tasks in the console. In this case, we're using it to run all the database migrations that haven't yet been run.

Now you can have a look at your work so far. Go to the browser and type `/works` after the address in the URL.

## Make this our home page

At the moment, "Welcome aboard" is occupying our important home page spot. Let's change things so that our works page shows up when we go to our website's root.

\begin{aside}
\label{aside:mvc}
\heading{Model View Controller}
\noindent

Rails likes to keep everything in the right place. It helps developers enormously to know that if you open up someone's Rails app you're going to know where to go to find the html.erb view files, where to go to find the database migrations, where the business logic is, where the stylesheets and javascript files will be, and so on. The structural conventions that Rails uses saves developers time -- and therefore money -- every day.

The structure that Rails apps use follow the MVC pattern [(Wikipedia page)](http://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller).

`Routing` decides which controller receives which requests. A `controller's` purpose is to receive specific requests for the application. A controller contains methods called actions, and each action's purpose is to collect information to provide it to a `view`.

If you use the scaffold, like we did, then Rails controllers come with the following actions. Open up `app/controllers/works/works_controller.rb` as well to follow along.

* index
* show
* new
* edit
* create
* update
* destroy

For the first four of these actions, there is a corresponding view file. Have a look in `app/views/works` and see. The final three are triggered when a user sends an instruction from the view -- for instance, to delete a record. In the case of a 'delete' instruction, the `destroy` controller action deletes the record then sends a message back to the relevant view, saying the record was deleted OK. There's no separate view for such an action.

A `view's` purpose is to display the information from a controller in a human readable format. An important distinction to make is that it is the `controller`, not the `view,` where information is collected. The `view` should only display that information.

(adapted from the [Rails Guides](http://guides.rubyonrails.org/)).

We used the Rails scaffold to create all the model, view, controller and routing files at the same time as the database table. You can create them one by one, as well.

\end{aside}


Open the file `app/config/routes.rb` in Nitrous.

This is your application's routing file which holds entries in a special DSL (domain-specific language) that tells Rails how to connect incoming requests to controllers and actions. This file contains many sample routes on commented lines. Add the following in, on the second line of the file:

```
  root 'works#index'
```

`root 'works#index'` tells Rails to map requests to the root of the application to the works controller's index action. `resources :works`
 tells Rails to map requests to http://localhost:3000/works/index to the works controller's index action. This was created earlier when you ran the generator (`rails generate scaffold work`).

Navigate to your home page in your browser. You'll see the works index page there, indicating that this new route is indeed going to the work controller's index action and is rendering the view correctly.

Read more about routing in the [Rails Guides](http://guides.rubyonrails.org/getting_started.html).
