# Making it look good

## Adding some styling

Your new `works` page is a bit ugly, to be honest, so let's add some styling. There is a free, widely-used design resource called Twitter Bootstrap. We’ll use that to add some styling to our app by amending our application's template. 

Open `app/views/layouts/application.html.erb` in your text editor. 

\begin{aside}
\label{aside:erb}
\heading{Why is that file named like that?}
\noindent 

ERB stands for Embedded Ruby. You can take a normal HTML file -- which would be called application.html, in this case -- and if you rename it to application.html.erb, you can embed snippets of Ruby code into it. The code snippets get run when the page is loaded in the browser. 

This comes in handy the more data you have. Say you have fifty books in your system. Instead of creating 50 html files, you can create one html.erb file and use Ruby to get the right data for each book, on the fly, when the browser asks for it. It's like using a template in Word, or any program: ERB saves time and duplication. 

\end{aside}

The `app/views/layouts/application.html.erb` file lays out the basic template for every page in our application. If you have seen any HTML before, most of this page will look familiar. HTML, or HyperText Markup Language, is the standard markup language used to create Web pages. HTML is written in the form of HTML elements consisting of tags enclosed in angle brackets (like <html> ). Read more in the [Wikipedia article](http://en.wikipedia.org/wiki/HTML) on HTML. There are lots of other online resources about HTML, but we can see the gist of how it works by looking at `application.html.erb`. 

If you look at that file, then go to your browser and click on `View > Developer > View source` (in Chrome. It's a similar path for other browsers), you'll see that the code is practically the same. Both versions start with

```html
<!DOCTYPE html>
<html>
<head>
  <title>BookOrganiser</title>
```


But see that `<%= stylesheet_link_tag %>` in the next line? That is a bit of Ruby code. We know it's Ruby because of the `<%=` at the beginning and the `%>` at the end. Anything between those two markers is Ruby, and will get evaluated. The `<%= stylesheet_link_tag %>` is the bit of Ruby code that will get evaluated. So the browser won't just show the phrase 'stylesheet_link_tag', or think that it's plain old HTML: some Ruby code will run instead.

This `stylesheet_link_tag` phrase is the name of a method -- a little program that Rails understands. In this case, the code that runs makes sure that our Rails app references the right Cascading Stylesheets, or CSS. (The CSS makes the page look the way it does in the browser.) So when you View Source in your browser you'll see HTML code that isn't in the `application.html.erb` file. That code has been inserted dynamically. 


Let's get on with enhancing how our app looks. Above the line

```
  <%= stylesheet_link_tag "application", media: "all", "data-turbolinks-track" => true %>
```

add

```html
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.1/css/bootstrap.min.css">
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.1/css/bootstrap-theme.min.css">
  <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.1/js/bootstrap.min.js"></script>
  <link href='http://fonts.googleapis.com/css?family=Oxygen:400,700,300' rel='stylesheet' type='text/css'>
  
```

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

\begin{aside}
\label{aside:dry}
\heading{Principle: Don't Repeat Yourself (DRY)}
\noindent 

`yield` identifies a section where content from the view should be inserted. This means we don't have to put all the header code and the nav bar code on every page. The things that will appear on every page can be coded once, and we can just insert the unique code for the different pages. This means our code is DRY. 

DRY is a principle of software development which states that "Every piece of knowledge must have a single, unambiguous, authoritative representation within a system." By not writing the same information over and over again, our code is more maintainable, more extensible, and less buggy.


\end{aside}

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
This adds a 'class' attribute called `table` to the HTML tag also called table. The `table` class is recognised by Twitter Bootstrap, and it makes the table take on Bootstrap's styling. It will give it nice padding and spacing, and some pleasing lines. (The people who wrote Bootstrap must have thought that giving the same name to the class as the tag was a good idea.)

Now make sure you saved your files and refresh the browser to see what has changed.

Let's add a bit of data. Click on `New Work` and use your shiny new website's form to add a new work to your system. Use Amazon to grab a cover image and a description. Type in the subject, but leave the cover field blank for now. Click on 'Create Work' when you're done.  

Great! You've not only created a web application, but you've added some real data to it. Let's see what else we can make it do. 
