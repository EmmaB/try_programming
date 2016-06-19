# Making it useful



Click on the Show link from the works page for one of your books. We've got some data in but we haven't got it looking very compelling yet. Let's amend this show page so that it makes our data look good.

But where did this show page come from?

\begin{aside}
\label{aside:dry}
\heading{Principle: Convention Over Configuration}
\noindent

Rails has opinions about the best way to do many things in a web application, and defaults to this set of conventions, rather than require that you specify every last thing. So when we used the Rails scaffold to create our works table, Rails also set up a bunch of files that most web developers would create, most of the time.

Go to the `app/views/works` folder. In there you'll see

* an Index ERB page
* a Show ERB page
* an Edit ERB page
* a New ERB page
* an ERB form.

Did you wonder why you were able to create a work without writing any code to define the form? Rails took an educated guess about what might be useful, and built it for you. Most of the time we like to see a summary listing of our data: that's taken care of by the Index page. We tend to want to create new data -- that's the New page. When the data changes, that's the Edit page. If we just want to read the data, we go to the Show page.  Rails has also set things up so we can delete data too, which is another common action -- we'll see that in a bit.

We refer to these common actions as CRUD: create, read, update and delete.  

\end{aside}

Did you see that the 'form' file starts with an underscore? Files that start with an underscore are called 'partials'. They contain snippets of code that you can easily reference from other files. It's that DRY principle at work again -- don't repeat yourself. The same form is used on the New page and the Edit page so we can reuse the code from one partial, to avoid having to have two versions of the same code to maintain.

Open `app/views/works/new.html.erb` to see what I mean. The file contains just three lines of code:

```erb
   <h1>New work</h1>
   <%= render 'form' %>
   <%= link_to 'Back', works_path %>
```

Type `/works/new` into your browser address bar to see the page that this code produces.

Where did the navigation bar come from?  It's not in our three lines of code. Well, do you remember looking at `application.html.erb`? The 'new' page uses the code produced by that template. Yep: the nav bar comes from the `application.html.erb` template. And where it said `yield` in that file, Rails is inserting the contents of the `new.html.erb` file. And even better -- where it says `render` on line 2, here, Rails knows to use the code from the partial called 'form'. `render`, similar to `yield`, means 'go and get some code from another file and pop it in here'.

Re-using code like this keeps everything neat and tidy and less cluttered.

## The show page

Let's look at the Show page. Click on `app/views/works/show.html.erb`. The first bit of Ruby that catches my eye is this line:

```ruby
  <%= @work.name %>
```

We know it's Ruby: we recognise the `<%=  %>` opening and closing ERB tags. What's the `@work.name`, though?

Look in the browser to see if we can figure it out. Go to your /works index page and click on the Show link. At the top it says "Name:" and then the name of your work. So this embedded Ruby must be getting the name of the work, somehow.

Working backwards, we can probably figure out that putting `.name` after `@work` returns some data. In this case, `.name` is a method. It's one of the columns we set up in the database, and Rails knows that when we use one of the column names, we mean 'get the data from that so-called column'. So if you look a bit further down the page, and see `@work.description`, you can be pretty confident that Rails knows to get whatever's in the column called 'description'.

But for which work? There might be 50 in the database. So Rails uses the unique ID that's included in the URL to look up the right one. Look up in the browser URL address bar. It says something like `https://metadata-manager-emmab.c9users.io/works/1`. Focus on the /works/1 bit for the moment. We can use another Rails convention: when the URL says [table name]/[number], in this case works/1, Rails knows to look in the works table for the record with ID 1. If the URL said /works/231, Rails would look up the record in the works table with ID 231.

It's not the view that figures all this out, though. The view gets given the @work instance variable from somewhere else: the controller. Here's the code that the controller uses to get the data from the database:

```ruby
  @work = Work.find(params[:id])
```

This is a Ruby query. It says:

`@work =`
create an instance variable called @work, and make it represent a particular works record. To find out which work record it is...

`Work`
...go to the works table...

`.find`
...and find...

`(params[:id])`
...the work whose ID is the one just sent from the webpage.

So if the params are /works/314, then we know the value of `params[:id]` is 314.

We can see the contents of params if we look on the console. Open the console window that has the server running in it. Scroll to the bottom, and then in your browser click on 'Show' again. In the console it will say something like:

```
  Started GET "/works/1" for 146.200.145.192 at 2015-01-17 18:51:23 +0000
  Processing by WorksController#show as HTML
  Parameters: {"id"=>"1"}
  Work Load (0.3ms)  SELECT  "works".* FROM "works"  WHERE "works"."id" = $1 LIMIT 1  [["id", 1]]
  Rendered works/show.html.erb within layouts/application (1.8ms)
  Completed 200 OK in 53ms (Views: 51.1ms | ActiveRecord: 0.3ms)
```

This is all very useful information:

```
  Started GET "/works/1" for 146.200.145.192 at 2015-01-17 18:51:23 +0000
```
Tells us that the browser sent a GET request for a work of ID 1.
```

  Processing by WorksController#show as HTML
```
tells us that this request got routed to the correct controller.   
```

  Parameters: {"id"=>"1"}
```
tells us that the params passed along with this request are `{"id"=>"1"}`
```
  Work Load (0.3ms)  SELECT  "works".* FROM "works"  WHERE "works"."id" = $1 LIMIT 1  [["id", 1]]
```
is the actual SQL query that runs to get the right data from the database.
```
  Rendered works/show.html.erb within layouts/application (1.8ms)
```
tells us that after the request was completed, the controller routed us back to the show page.
```
  Completed 200 OK in 53ms (Views: 51.1ms | ActiveRecord: 0.3ms)
```
tells us that all of this took less than half a second.


The params (which is short for 'parameters') are passed from the view to the controller as a "hash". Ruby hashes are one of the things that programmers love the most about the language because they're useful and flexible. They contain data in key-value pairs, in any order, like this:

    person = {:name => "Emma", :age => "40", :hobby => "piano"}

You can find out much more about hashes on the internet. For now, you've seen that you can get the value of data in a hash if you say the name of the hash and then put the key of the data you want, in square brackets.

So in this example, doing `person[:name]` would return "Emma". `person[:hobby]` would return -- yep, you guessed it -- "piano". How would you get my age? `person[:age]`. (Let's look at Ruby arithmetic, later on, to knock some years off.)

In our real example, doing `params[:id]` would return: 1.


## Creating an AI

Now we know how to get data from the database, and channel it through the right routes to get to the webpage view, let's try to make some of that data look pretty.

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

That new code uses `@work.cover_url`, `@work.name`, `@work.subject`, and `@work.description`. It's also arranged the page into two columns, using the Twitter Bootstrap styling we included. Bootstrap uses a grid layout of 12 columns, so if we set up one div of 4 columns and one of 8 columns, we know it'll fit nicely on the page.

The page is looking pretty nice so far. However, we've only got the work level data. We're really going to need to see the actual products' information if anyone's going to be able to use this to place orders. Let's add a products table.

## Adding products to our works

Open a new console window and run the following:

```
  rails generate scaffold Product isbn:string pub_date:date format:string price:decimal pages:integer work_id:integer
```

then
```
  rake db:migrate
```

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

In the view file `app/views/products/_form.html.erb`, replace

```
    <%= f.number_field :work_id %>
```


With

```
    <%= f.collection_select :work_id, Work.all, :id, :name %>
```

`collection_select` is a Rails helper method. This method takes four arguments. It's going to let us pass the value of work_id into the params when we hit 'save'. It'll get the ID from the dropdown list, which will be populated by Work.all (all the works). The next argument says that it's the id that'll be passed. And finally, the last argument tells Rails that it should display the name of the work to the user of the website.

Go to the browser and see that there's a drop down list now. Create a couple of products using the form.

You'll see that we can put any old thing into the ISBN field. Let's use the gem we added earlier to check whether the ISBN is properly formed.

In `app/views/products/_form.html.erb` add this code in just under the ISBN field div:

```erb
<% if ISBN.valid?(@product.isbn) %>
  <p class = 'bg-success'>Great -- the ISBN is valid!</p>
<% elsif @product.isbn.blank? %>
  <p class = 'bg-info'>Don't forget to add an ISBN!</p>
<% else %>
  <p class = 'bg-danger'> Oh no -- the ISBN is not valid!</p>
<% end %>
```

The `ISBN` method comes as part of the `isbn` gem that we included earlier. It's quite readable, which is one of the principles of Ruby. Method names should be easy to read and understand. You can practically say out loud in English what's going on:

"If the ISBN is valid -- the product's ISBN -- then say great, and put some green on the screen to signify that all's well. Otherwise, if the product's ISBN is blank, put up an information message to remind the user to add an ISBN. If the ISBN is neither valid nor missing, then it must be invalid, so report the problem to the user." In fact the Ruby is much easier to read than that sentence in English.

Now, let's make the products appear on the work show page. Go to `application/views/works/show.html.erb`. After the description field, paste this in:

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

That block of code is going to run for each of the child products of the parent work. We don't have to have a piece of code for each product -- the code gets reused.

## Other forms of the same data: APIs

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


Now go to the works listing page, and in the address bar of the browser, add `.xml` after the number. You'll see something like this:



```xml
<works type="array">
  <work>
    <id type="integer">1</id>
    <name>War and Peace</name>
    <description>
      War and Peace (Pre-reform Russian: Война и миръ, Voyna i mir) is a novel by the Russian author Leo Tolstoy, first published in 1869. The work is epic in scale and is regarded as one of the most important works of world literature.[1][2][3] It is considered as Tolstoy's finest literary achievement, along with his other major prose work, Anna Karenina (1873–1877). War and Peace delineates in graphic detail events surrounding the French invasion of Russia, and the impact of the Napoleonic era on Tsarist society, as seen through the eyes of five Russian aristocratic families. Portions of an earlier version of the novel, then known as The Year 1805,[4] were serialized in the magazine The Russian Messenger between 1865 and 1867. The novel was first published in its entirety in 1869.[5] Newsweek in 2009 ranked it first in its list of the Top 100 Books.[6] In 2003, the novel was listed at number 20 on the BBC's survey The Big Read.[7] Tolstoy himself, somewhat enigmatically, said of War and Peace that it was "not a novel, even less is it a poem, and still less a historical chronicle". Large sections of the work, especially in the later chapters, are philosophical discussion rather than narrative.[8] He went on to elaborate that the best Russian literature does not conform to standard norms and hence hesitated to call War and Peace a novel. (Instead, Tolstoy regarded Anna Karenina as his first true novel.)
    </description>
    <cover>
      <url>/uploads/work/cover/1/9781909679436.jpg</url>
    </cover>
    <subject>Fiction</subject>
    <created-at type="dateTime">2015-01-17T14:14:45Z</created-at>
    <updated-at type="dateTime">2015-01-17T14:46:07Z</updated-at>
  </work>
</works>
```

Do the same but now appending `.json`. You'll see something like this:

```xml
[{"id":1,"name":"War and Peace","description":"War and Peace (Pre-reform Russian: Война и миръ, Voyna i mir) is a novel by the Russian author Leo Tolstoy, first published in 1869. The work is epic in scale and is regarded as one of the most important works of world literature.[1][2][3] It is considered as Tolstoy's finest literary achievement, along with his other major prose work, Anna Karenina (1873–1877).\r\n\r\nWar and Peace delineates in graphic detail events surrounding the French invasion of Russia, and the impact of the Napoleonic era on Tsarist society, as seen through the eyes of five Russian aristocratic families. Portions of an earlier version of the novel, then known as The Year 1805,[4] were serialized in the magazine The Russian Messenger between 1865 and 1867. The novel was first published in its entirety in 1869.[5] Newsweek in 2009 ranked it first in its list of the Top 100 Books.[6] In 2003, the novel was listed at number 20 on the BBC's survey The Big Read.[7]\r\n\r\nTolstoy himself, somewhat enigmatically, said of War and Peace that it was \"not a novel, even less is it a poem, and still less a historical chronicle\". Large sections of the work, especially in the later chapters, are philosophical discussion rather than narrative.[8] He went on to elaborate that the best Russian literature does not conform to standard norms and hence hesitated to call War and Peace a novel. (Instead, Tolstoy regarded Anna Karenina as his first true novel.)","cover":{"url":"/uploads/work/cover/1/9781909679436.jpg"},"subject":"Fiction","created_at":"2015-01-17T14:14:45.138Z","updated_at":"2015-01-17T14:46:07.203Z"},{"id":2,"name":"","description":"","cover":{"url":null},"subject":"","created_at":"2015-01-17T16:58:50.020Z","updated_at":"2015-01-17T16:58:50.020Z"}]
```

This is a JSON API. If this was live, other websites would be able to ping it and use its data. APIs are hugely powerful because they release data in a lightweight format for apps to search, ingest and discover. [Here's](http://publishingperspectives.com/2011/04/publishing-importance-of-api/) a good article on them, and [another](http://toc.oreilly.com/2013/02/a-publishers-job-is-to-provide-a-good-api-for-books.html).

Go to `https://www.hurl.it/` and paste the full URL of your works.json page in, e.g.

```
http://supersonic-ghost-95-183846.euw1-2.nitrousbox.com/works.json
```

The page will show that your API has parsed successfully.
