# Getting started
\label{cha:a_chapter}


## A recent history of code development.

There are lots of different programming languages, and more are being created all the time. They are often very specialised. COBOL was designed to make it easy to solve business problems. FORTRAN was, and still is, used to write high-performance scientific programs.

[Here](http://en.wikipedia.org/wiki/List_of_programming_languages) is a list of notable languages.



In this course we’ll be using a language called Ruby, which has been around for about 20 years. Ruby became very popular as a language for developing web-based applications about ten years ago, after the release of an “application development framework” written in Ruby, called “Ruby on Rails”. The aim of Ruby on Rails was to make it very easy to develop web based applications by hiding nearly all of the complexity from the developer.

Over the past ten years, many more people have contributed further to the ease of developing Ruby on Rails applications by contributing more open source code to help with specific common tasks. These contributions, known as “gems”, help with tasks such as providing password-based access to systems, validating ISBNs, or allowing users of the system to upload files such as images or documents through the application.

So the trend over the past decade has been to make it easier and easier to solve common tasks by just using code that other people have contributed, making it more simple to write sophisticated applications.

When the application has been written, it used to be the case that you then had to buy or rent a “server” that is connected to the internet, and then install and maintain all of the required software and security configuration. However another major change has been a recent profusion in companies, such as Amazon and Google, offering rental of their own computers for running code and managing applications. Other companies, such as Netflix, run their businesses entirely on rented servers. Furthermore, yet other companies provide management layers on top of these services that hide their complexity from you, making it even easier to run your own application, and because they have a huge customer base of very similar systems, they can do so extremely cheaply.

Until very recently this still left you with the problem of setting up your own computer to develop your application, and this has often been a rather trying business. In the past year or two, other companies have developed “hosted” systems that allow you to develop your code on their servers, so you don’t even need to have a sophisticated and complex development environment on your own desktop or laptop computers. Today we will be using a service called “Nitrous” to develop our code.




## Set up a Nitrous.io account and a new container

* Go to https://www.nitrous.io/
* Enter a username.
* Now enter your email address.
* Enter a password.
* Hit "Sign up for free". If any of your details aren't up to scratch, have another go.

## Create a development box

* On the New Container page, select Ruby on Rails, and hit Next.
* Select Europe as your region
* Select Starter (free) as your plan and hit Create.
* Sadly you'll have to provide your phone number for a free plan. Enter it and hit Save, then enter your verification code once your SMS has arrived.
* Leave the box labelled "Open the IDE when ready" checked, and wait whilst Nitrous sets up your new box.
* Select Get Started once that's an option on the screen.

## Logging back in

If your login expires, you can log back in with your user name and password.

Click on `Start` and then `IDE` (which stands for Interactive Development Environment).

## A look around Nitrous

At the top of the page, the screen is divided into two. On the left is a file system navigator -- just like you’d see in Finder on the Mac or Windows Explorer on a PC.

On the right is the contents of whichever file is open on Nitrous.

At the bottom of the page you have a console. It’s the same as what you’d find if you’ve ever opened the Terminal on your Mac, or the Command Prompt on Windows. You can enter commands here which the computer will obey. Using the command line is often a lot quicker and more reliable than using the graphical user interface of your machine.

## Create a Ruby on Rails application  

Oh my! We're ready to create your first Ruby on Rails application. But what does that mean?

\begin{aside}
\label{aside:what_is_an_application}
\heading{What is a Rails application?}
\noindent

When you go to the Scribd website, or the Yellow Pages website, or Airbnb, Groupon or Bloomberg, you're looking at Ruby on Rails applications.

When you run the code of a Rails application, it generates HTML pages. Unlike a static HTML website, though, a Rails app can display different data, drawn from a database, depending on what the website user clicks on and types.

So: a Rails application generates HTML pages on the fly. And what are a bunch of HTML pages usually called? A website.

\end{aside}

Let’s set up our new Rails app in the correct folder on Nitrous. This next command is the same as going to Finder and clicking on a folder. But you’re a programmer now, so you can use the command line. ‘cd’ stands for ‘change directory’. In the console, click near the $ dollar sign, which is known as the ‘command prompt’, and type:  
```
    cd code
```
The console command prompt changes to show that you’ve changed into the directory folder called ‘code’.

You know when you install an app or a new software program that you've download onto your phone or computer?  You need to do the same for the database program that we'll be using. We'll be using PostgreSQL for our database. Nitrous has configured that for you already.

We'll call our Rails app "Book Organiser". What we're aiming for is to build a website that will appear somewhere like `www.bookorganiser.com`. To create your new Rails app, run the following command:

```
    rails new book_organiser -d postgresql
```

That's our database program installed and running, and our Rails app created, and we've only typed three commands! Next, we need to connect your new Rails app to a PostgreSQL database.
Switch to the directory containing your new app:
```
    cd book_organiser    
```

Next, find the file at `code/book_organiser/Gemfile`. Click on it, and its contents will appear in the right hand pane. We're going to run this code. Go back in the console, and run the following command:

```
  bundle install
```

Now, find the file at `code/book_organiser/config/database.yml`. Click on it, and its contents will appear in the right hand pane. We're going to run this code. Go back in the console, and run the following command:

```
  rake db:create db:migrate
```

OK! We've created a new Rails app, created a postgresql database and connected our app to our database. We’re ready to start up your new Rails app. Run the following command:

```ruby
  rails s -b 0.0.0.0
```
or
```
  ~/code/book_organiser/start-app
```

Later, you can stop the server with Control-c to return to the command prompt.

Then on Nitrous’s navigation menu at the top of the page, click on `Preview > Port 3000`. You'll see a website declaring "Welcome aboard.
You’re riding Ruby on Rails!"

You’ve done it! Your new Rails application is running.
