# Further Exercises

Here are some ideas for more ways you could improve your app. Can you: 

* make a thumbnail image appear on the Works listing page, instead of the URL? 
* change the links on the Works page so that they say "AI", "Edit", and "Remove", instead of the boilerplate "Show", "Edit" and "Destroy"?
* add a link on the works listing page to switch to the XML view? Here's the Rails syntax for a link: `<%= link_to "XML", works_path(:format => :xml) %>`
* add a link to the work from the product page?  Here's the Rails syntax for the link: `<%= link_to "Work", work_path(@product.work) %>`

* Look at the [Bootstrap documents](http://getbootstrap.com). Are there any ways you could alter the styling of the HTML pages, using classes that Bootstrap responds to? 
* create an Author model and associate it with the work? 
* generate a JSON API response for a single work? 
