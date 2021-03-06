## Review ERB syntax

Now in Ruby on Rails you have a new type of files `.erb` and this type allows you to add Ruby code inside "ERB tags”. You can and have to still use the exact same HTML syntax you have learned before. Nothing changed in terms of that. HTML looks still exactly the same way. Only **in addition to** the HTML code you also have a new type of tags: ERB tags. ERB tags actually come in two different versions:  `<% … %>` and `<%= %>`
Inside those tags you have to write pure Ruby code. **Outside** of them you can NOT use Ruby code and you have to always stick  to normal HTML code syntax (as we talked about above).
Ruby is a programming language with features like loops, conditionals, variables, objects and so on. You learned about Ruby before. With ERB tags you are able to write Ruby logic in the middle of your HTML code. That’s particularly  useful to access variables and that’s what you are doing on your Rails page.
But first let’s get back to the two types of ERB tags:

- `<% … %>` will run everything inside of it but NOT output anything on the HTML page. What this could look like practically I’m going to show you in just a minute:
- `<%= … %>` on the other hand will run the code inside of it and then output the result on the HTML page in the middle of the rest of your HTML code.
So what does that look like? On any of your ERB pages (doesn’t matter which one. Take for example views/static_pages/index.html.erb) add the following ERB code at the beginning:

```
<% test_variable = 42 %>
```

This is standard Ruby code as you learned it before. It defines a variable called `text_variable` and assigns the integer `42` to it. So far so good. Refresh the page. What do you see? Nothing changed! That’s because you used the ERB tag, that will only run but NOT be displayed in your HTML. So let’s display the variable we just defined. Under the ERB tag you just wrote add:

```
<%= test_variable %>
```

Refresh the page. Now you should see a `42` on your page somewhere. That’s because the ERB code we used generates an output and puts it on our website. The output in this case is the number 42. You can even make calculations like that:

```
<%= test_variable - 2 %>
```

This will output 40 on your page. This is still all very basic Ruby code and you can use it in the middle of your HTML page using ERB tags!

## Model-View-Controller

Now let’s get back to Rails. Rails is a huge framework written in Ruby. It does a lot of things in the background for you, so you don’t have to worry about it. It sometimes does so much stuff, that you can even be confused as why and how things even work at all. But in the end Rails makes things much easier. And the more you work with it, over time, you will see that and get used to it and understand a little more every time you use it.

Your app has a database. That’s where you can dynamically store data. That’s how all sorts of apps work. Let’s take a ToDo-List app for example. It has a table in the database called "tasks" for example. And the columns are maybe task_title (which is a string) and task_done (which is a boolean). Now the app probably has a form with two input fields (one for each of these columns). The first one is a text field and the user can type in the title of the task (and it will be saved as a string). The second one is a checkbox which the user can tick or un-tick depending on whether or not the task is done (this will be saved as a boolean. The value of the boolean is true if the checkbox is ticked and the task is done). All the tasks of the user are stored in a table inside the database of the app. Just like your products table. That stores all the products shown on the website.

Rails does a lot of magic in the background to make it easy for us to access and work with that data stored in the database. And that’s what the “Model” is for. Models are Rails’ way of accessing that data. Rails will “magically” turn your database table in an object and store it in a variable. So for example if you use scaffolding to create a database table called `Tasks`, you could access all tasks in Rails using `Task.all`. The same way you can access the products in the products table on your website by typing in `Product.all` (it’s important that it’s capital and singular. That’s just the Rails way to do it and you have to follow it. All Model variables are capitalized and singular).
Go to the folder **app/controllers** and open `products_controller.rb`. In there are different Ruby methods (in Rails all the methods inside a controller are also called “actions” but they are exactly the same as methods). They represent the different products pages. So the variables you define inside the `def index` action will be available for use on the **app/views/products/index.html.erb** page but NOT on show.html.erb (that’s what the `def show` action is for).
If you look in there you see `@products = Product.all`
This is Ruby code you know now! The variable `@products` is defined to equal `Products.all`. And from what I just told you, you know now, that because this variable is defined inside the `index` action, you can use the variable inside your index.html.erb page of your products folder.
You can try what this means. You can add your own variable like this:

```
  def index
    @products = Product.all
    @test_variable = 42
  end
```

Then in your **index.html.erb** page put somewhere (no matter where) `<%= @test_variable %>`
See how 42 is now displayed on your page?
Alright. Now we are getting closer. So `Product.all`, which is represented by `@products` will return an array of objects. So if you just wrote `<%= @products %>` in your HTML code, you would actually see that array. That looks obviously not really nice. That’s why Rails, by default, generated a **Ruby loop** for you: `<% @products.each do |product| %> ... <% end %>`
This loop will iterate over all individual objects inside the Product array. And during each iteration the current object will have the variable `product` (that’s defined here: `|product|`). Now everything between the beginning of the loop and the `<% end %>` tag will run as often as the number of product objects in the array. So if there are none, the loop won’t run at all. If you only added one product, the loop will run ones.
This also means that everything inside that loop will be added to the HTML page as often as the loop runs. Remove everything inside the loop and try this:

```
<%= @products.each do |product| %>
  This is text<br>
<% end %>
```

If you have no products in the database, you will actually not see `This is text` on your website at all. If you, however, added one ore more products already, you will see this line of text now underneath each other as often as the amount of products in your table.
To access the name of each product you can now use the variable we defined earlier. And since it is an object we can use dot-notation to access all the properties. (Remember: you defined your product to have the properties name, description and image_url).
So you can write:

```
<%= @products.each do |product| %>
  <%= product.name %>
<% end %>
```

If you refresh the page now, you will notice, how all the product names are displayed. But they are in the same line and no space in between. That doesn’t look very great. So now you can add HTML code to style it. For example like this:

```
<%= @products.each do |product| %>
  <h2><%= product.name %></h2>
<% end %>
```

And you can even add classes:

```
<%= @products.each do |product| %>
  <h2 class=“class1 class2”><%= product.name %></h2>
<% end %>
```

and the description:

```
<%= @products.each do |product| %>
  <h2 class=“class1 class2”><%= product.name %></h2>
  <p><%= product.description %></p>
<% end %>
```
