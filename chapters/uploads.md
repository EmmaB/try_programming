# Adding more features

## Adding  uploads


You can judge a book by it's cover -- and we'll need to be able to manage the covers of our books. We're going to use a bundle of pre-written code called a "gem" to let us upload files to our Rails app.

Open the file called `Gemfile` in the project directory and at the bottom add

```
   gem 'carrierwave'
   gem 'isbn'
```

and save the file.

\begin{aside}
\label{aside:open_source}
\heading{Why is the carrierwave and the isbn code free?}
\noindent

Rails is an open source library of code, known as a "gem". There are many different gems, created by developers around the world, made freely available as part of the open source software movement. There's a website called [rubygems.org](https://rubygems.org/) which acts as a reference for all the gems published. You can usually see the gem's source code on code repository service [github.com](http://github.com).

The Ruby community encourages its members to open-source and contribute to as much high-quality gems as they can, so that everyone can benefit from code that has had many different developers working on it.

We've included two gems here. The first is for uploading files. The second is an ISBN validations testing library. We'll look at them  in turn.

\end{aside}


In the console run:

```
bundle
```

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

Sometimes, you might get an TypeError: can’t cast ActionDispatch::Http::UploadedFile to string.

If this happens, in `file app/views/works/_form.html.erb` change the line

```
<%= form_for(@work) do |f| %>
```

to

```
<%= form_for @work, :html => {:multipart => true} do |f| %>
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
