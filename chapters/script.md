# Code, instructions and coach notes

This chapter contains only the instructions and the code that we'll be using on the course, and some brief coaching notes. The content is the same as in the previous chapters, summarised here for convenience during the course.

## Chapter 1: Getting started

\begin{aside}
\label{aside:chapter1}
\heading{Coach notes}
\noindent

Talk about the recent history of programming languages, and how nowadays there are tools to get started in programming without too much fuss.

\end{aside}


## Set up a Cloud9 account

* Go to https://c9.io/
* Enter your email address
* Enter your name
* Enter a user name
* Select Hobbyist and Hobby Projects
* Check the box to show you're not a robot and click "Create Account"


## Create a workspace

* On the page that loads, click "Create a new workspace"
* Give your project the name "book_organiser"
* Give your project a description, such as "Bibliographic data management app"
* Leave Public selected
* Click Ruby on Rails in the "select a template" area
* Click "Create workspace"
* After a while your IDE (interactive development environment) will load with a freshly-created Ruby on Rails app.


## Chapter 2: Structuring our app

\begin{aside}
\label{aside:chapter2}
\heading{Coach notes}
\noindent

Talk about MVC and object oriented programming.

\end{aside}


```
  cd book_organiser
```

\begin{aside}
\label{aside:chapter2a}
\heading{Coach notes}
\noindent

Talk about Rails scaffolding, and data types.

\end{aside}


```
  rails generate scaffold work name:string description:text cover:string subject:string
```

```
  rake db:migrate
```

\begin{aside}
\label{aside:chapter2b}
\heading{Coach notes}
\noindent

Talk some more about MVC, and particularly routing.

\end{aside}


Open the file `app/config/routes.rb` in Cloud 9. Add the following in, on the second line of the file:

```
  root 'works#index'
```

## Chapter 3: Making it look good

\begin{aside}
\label{aside:chapter3}
\heading{Coach notes}
\noindent

Talk about ERB.

\end{aside}

### Enhance how our app looks with Bootstrap

Let's get on with enhancing how our app looks. Above the line

```
  <%= stylesheet_link_tag "application", media: "all", "data-turbolinks-track" => true %>
```

add

```html
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css">
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap-theme.min.css">
  <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/js/bootstrap.min.js"></script>
  <link href='http://fonts.googleapis.com/css?family=Oxygen:400,700,300' rel='stylesheet' type='text/css'>

```

### Keep things DRY

\begin{aside}
\label{aside:chapter3a}
\heading{Coach notes}
\noindent

Talk about the DRY principle.

\end{aside}


Next, look for

```erb
  <%= yield %>
```

and replace it with

```erb
<div class="container">
  <%= yield %>
</div>
```

Let’s also add a navigation bar to the layout. In the same file, under `<body>`, add

```html
<nav class="navbar navbar-default navbar-fixed-top navbar-inverse" role="navigation">
  <div class="container">
    <div class="navbar-header">
      <button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
        <span class="sr-only">Toggle navigation</span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
      </button>
      <a class="navbar-brand" href="/">The Book Organiser app</a>
    </div>
    <div class="collapse navbar-collapse">
      <ul class="nav navbar-nav">
        <li><a href="/works">Works</a></li>
      </ul>
    </div>
  </div>
</nav>
```
Now let’s also change the styling of the Works table. Open `app/assets/stylesheets/application.css` and at the bottom add

```css
body { padding-top: 100px;   font-family: Oxygen, sans serif}
footer { margin-top: 100px; }
table, td, th { vertical-align: middle; border: none; }
th { border-bottom: 1px solid #DDD; }
```

Next, `open app/views/works/index.html.erb` and replace line 3:

```
<table>
```
with
```
<table class ='table'>
```
### Add a work

Let's add a bit of data. Click on `New Work` and use your shiny new website's form to add a new work to your system. Use Amazon to grab a cover image and a description. Type in the subject, but leave the cover field blank for now. Click on 'Create Work' when you're done.  

## Chapter 4: Adding more features

### Adding gems

\begin{aside}
\label{aside:chapter4a}
\heading{Coach notes}
\noindent

Talk about the open source movement and gems.

\end{aside}


Open the file called `Gemfile` in the project directory and at the bottom add

```
   gem 'carrierwave'
   gem 'isbn'
   gem 'haml'

```

and save the file.

In the console run:

```
bundle
```

### Adding uploads


Now we can generate the code for handling uploads. In the terminal run:

```
rails generate uploader Cover
```

At this point you need to restart the Rails server process in the console. Hit CTRL-C in the terminal to quit the server. Once it has stopped, you can press the up arrow to get to the last command entered, then hit enter to start the server again. This is so the app can load the new code.

Open `app/models/work.rb` and under the line

```
class Work < ActiveRecord::Base
```

add

```
mount_uploader :cover, CoverUploader
```

Open `app/views/works/_form.html.erb` and change

```
<%= f.text_field :cover %>
```

to

```
<%= f.file_field :cover %>
```

In your browser, add a new work with a cover. When you upload a cover it doesn’t look nice because it only shows a path to the file, so let’s fix that.

Open `app/views/works/show.html.erb` and change

```
<%= @work.cover %>
```

to

```
<%= image_tag(@work.cover_url, :width => 300) if @work.cover.present? %>
```

Now refresh your browser to see what has changed.

### Improving the code


\begin{aside}
\label{aside:chapter4c}
\heading{Coach notes}
\noindent

Talk about Haml.

\end{aside}

Go to http://htmltohaml.com and paste the whole contents of `app/views/works/_form.html.erb` in.

Copy the Haml version on the right, then go to `app/views/works/_form.html.erb` and paste it all in. Then rename the file to be `_form.html.haml`.

Restart the server by going to the console where the server is running and hitting CTRL-C, then up-arrow to get the last command, which was `rails server`, then hitting enter. You'll see that nothing has changed on the browser page, but our code is easier to read and more elegant.

## Chapter 5: Making it useful

### Convention over configuration

\begin{aside}
\label{aside:chapter5}
\heading{Coach notes}
\noindent

Talk about convention over configuration.

\end{aside}

In `app/views/works/show.html.erb`, paste in this code, replacing everything that's in there already:

```erb
<p id="notice"><%= notice %></p>

<div class="row">
  <div class="col-sm-4">
    <%= image_tag(@work.cover_url, :width => 300) if @work.cover.present? %>
  </div>

  <div class="col-sm-8">
    <h1>
      <%= @work.name %>
    </h1>
      <strong>  
      <%= @work.subject %>
      &bull;  
    </strong>
    <%= simple_format @work.description %>
  </div>
</div>
<%= link_to 'Edit', edit_work_path(@work) %> |
<%= link_to 'Back', works_path %>
```

### Queries

\begin{aside}
\label{aside:chapter5a}
\heading{Coach notes}
\noindent

Talk about how methods and ActiveRecord queries work

\end{aside}

### Associations


Let's add in products. Open a new console window and run the following:

```
  rails generate scaffold Product isbn:string pub_date:date format:string price:decimal pages:integer work_id:integer
```

then
```
  rake db:migrate
```

\begin{aside}
\label{aside:chapter5b}
\heading{Coach notes}
\noindent

Talk about associations.

\end{aside}

Now we have to relate the products to their parent work, and vice versa. A work has many products, and a product belongs to a work. First we'll go to the work model and add in the association. Go to `app/models/work.rb`:

```
class Work < ActiveRecord::Base
  mount_uploader :cover, CoverUploader
end
```

Add the following in before the `end`.  

```
  has_many :products, :inverse_of => :work, :dependent => :destroy
```

And we add the reciprocal line in to the Product model:

Go to `app/models/product.rb`.

```
class Product < ActiveRecord::Base
end
```

Add the following in before the `end`.  

```
  belongs_to :work, :inverse_of => :products
```

Now let's add our new products listing to the navigation menu: find the following:

```
    <div class="collapse navbar-collapse">
      <ul class="nav navbar-nav">
        <li><a href="/works">Works</a></li>
      </ul>
    </div>
```
and add in this line:

```
  <li><a href="/products">Products</a></li>
```

so it reads:
```
    <div class="collapse navbar-collapse">
      <ul class="nav navbar-nav">
        <li><a href="/works">Works</a></li>
        <li><a href="/products">Products</a></li>
      </ul>
    </div>
```

Add `class = 'table'` to the products table. Go to `app/views/products/index.html.erb` and change line 3 to

```
<table class = 'table'>
```

Click New product. You can see the correct fields, but we can make it easier to fill them it accurately.


### Using form helpers



\begin{aside}
\label{aside:chapter5c}
\heading{Coach notes}
\noindent

Talk about form helpers and the other sorts of Rails methods that save time.

\end{aside}

Replace

```
    <%= f.number_field :work_id %>
```

With

```
    <%= f.collection_select :work_id, Work.all, :id, :name %>
```

Go to the browser and see that there's a drop down list now. Create a couple of products using the form.

### Conditional statements


\begin{aside}
\label{aside:chapter5e}
\heading{Coach notes}
\noindent

Talk about using the ISBN gem, conditional statements, the notion of validations and how Ruby is human-readable.

\end{aside}

In `app/views/products/edit.html.erb` add this code in just under the ISBN field div:

```erb
<% if ISBN.valid?(@product.isbn) %>
  <p class = 'bg-success'>Great -- the ISBN is valid!</p>
<% elsif @product.isbn.blank? %>
  <p class = 'bg-info'>Don't forget to add an ISBN!</p>
<% else %>
  <p class = 'bg-danger'> Oh no -- the ISBN is not valid!</p>
<% end %>
```

### Code blocks



\begin{aside}
\label{aside:chapter5d}
\heading{Coach notes}
\noindent

Talk about blocks.

\end{aside}

Now, let's make the products appear on the work show page. Go to `app/views/works/show.html.erb`. After the description field, paste this in:


```erb
  <% @work.products.each do |product| %>
    <p>
      <%= product.format %>
      &bull;  
      <%= product.isbn %>
      &bull;  
      <%= product.pages %> pages
      &bull;  
      £<%= product.price %>
      &bull;  
      Published <%= product.pub_date.strftime('%d %B %y') %>
    </p>
  <% end %>

```

### APIs


\begin{aside}
\label{aside:chapter5d}
\heading{Coach notes}
\noindent

Talk about APIs.  

\end{aside}


Replace the index action in the works controller with this:

```ruby
  def index
    @works = Work.all
    respond_to do |format|
      format.html
      format.xml { render :xml => @works.to_xml }
      format.json { render :json => @works.to_json }
    end
  end
```
