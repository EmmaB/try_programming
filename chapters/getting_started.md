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




## Set up a Cloud9 account

* Go to https://c9.io/
* Enter your email address
* Enter your name
* Enter a user name
* Select Hobbyist and Hobby Projects
* Check the box to show you're not a robot and click "Create Account"


## Create a workspace

* On the page that loads, click "Create a new workspace"
* Give your project a name, such as "metadata_manager" or something more inventive
* Give your project a description, such as "Bibliographic data management app"
* Leave Public selected
* Click Ruby on Rails in the "select a template" area
* Click "Create workspace"
* After a while your IDE (interactive development environment) will load with a freshly-created Ruby on Rails app.


## A look around C9

At the top of the page, the screen is divided into two. On the left is a file system navigator -- just like you’d see in Finder on the Mac or Windows Explorer on a PC.

On the right is the contents of whichever file is open on Cloud 9.

At the bottom of the page you have a console. It’s the same as what you’d find if you’ve ever opened the Terminal on your Mac, or the Command Prompt on Windows. You can enter commands here which the computer will obey. Using the command line is often a lot quicker and more reliable than using the graphical user interface of your machine.

## Look at your new Ruby on Rails application  

Oh my! We've created your first Ruby on Rails application. But what does that mean?

\begin{aside}
\label{aside:what_is_an_application}
\heading{What is a Rails application?}
\noindent

When you go to the Scribd website, or the Yellow Pages website, or Airbnb, Groupon or Bloomberg, you're looking at Ruby on Rails applications.

When you run the code of a Rails application, it generates HTML pages. Unlike a static HTML website, though, a Rails app can display different data, drawn from a database, depending on what the website user clicks on and types.

So: a Rails application generates HTML pages on the fly. And what are a bunch of HTML pages usually called? A website.

\end{aside}

On Cloud 9's navigation menu at the top of the page, click on `Run project`. You'll see a server start up in the lower pane. Click on Preview Running Application at the top, and then a new pane will open. That is a browser window, and in it you'll see a website declaring "Welcome aboard. You’re riding Ruby on Rails!"

You’ve done it! Your new Rails application is running.
