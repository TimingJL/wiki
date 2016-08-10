# Learning by Doing 

![Ubuntu version](https://img.shields.io/badge/Ubuntu-16.04%20LTS-orange.svg)
![Rails version](https://img.shields.io/badge/Rails-v5.0.0-blue.svg)
![Ruby version](https://img.shields.io/badge/Ruby-v2.3.1p112-red.svg)

Mackenzie Child's video really inspired me. So I decided to follow all of his rails video tutorial to learn how to build a web app. Through the video, I would try to build the web app by my self and record the courses step by step in text to facilitate the review.


# Project 9: How To Build A Wikipedia Clone With Rails       
This week we built a Wikipedia Type Application. We have the Users and they have ability to Add, Edit, Destroy articles postings and each article is assigned to a category, and we are able to filter between the various categories to see only articles within that category (For example, only “Art” or only “Technology” articles).


https://mackenziechild.me/12-in-12/9/         



### Highlights of this course
1. Users
2. Articles
3. Categories
4. HAML
5. Bootstrap


# Create A Wiki
```console
$ rails new wiki
```

Chage directory to the pin_board. Under `Gemfile`, add `gem 'therubyracer'`, save and run `bundle install`.      

Note: 
Because there is no Javascript interpreter for Rails on Ubuntu Operation System, we have to install `Node.js` or `therubyracer` to get the Javascript interpreter.

```console
$ bundle install
```

Then run the `rails server` and go to `http://localhost:3000` to make sure everything is correct.  


### Rubygems Install
First, we add a few rubygems that we're going to use.            

In `Gemfile`:
```
gem 'therubyracer'
gem 'devise'
gem 'simple_form', github: 'kesha-antonov/simple_form', branch: 'rails-5-0'
gem 'haml', '~> 4.0.5'
gem 'bootstrap-sass', '~> 3.2.0.2'
```

TO install `simple_form`, we need to run the generator:
```console
$ rails generate simple_form:install --bootstrap
```

### Add The Ability To Create AN Article
Next, we wanna generate a model and a controller for our article.
```consle
// Model
$ rails g model Article title:string content:text
$ rake db:migrate

// Controller
$ rails g controller Articles
```

Let's define an index action for our homepage in `app/controllers/articles_controller.rb`
```ruby
class ArticlesController < ApplicationController
	def index
	end
end
```

And open up our `config/routes.rb` and create some routes for our articles.
```
Rails.application.routes.draw do
  resources :articles
  root 'articles#index'
end
```

Let's create a new file under `app/views/articles` and save it as `index.html.haml`.
```haml
%h1 This is the articles#index placeholder
```


So let's add the ability to create a new article.          
In `app/controllers/articles_controller.rb`
```ruby
class ArticlesController < ApplicationController
	def index
	end

	def new
		@article = Article.new
	end

	def create
		@article = Article.new(article_params)
		if @article.save
			redirect_to @article
		else
			render 'new'
		end
	end

	private

	def article_params
		params.require(:article).permit(:title, :content)
	end
end
```

Under `app/views/articles`, let's create a `new` page and save it as `new.html.haml`. And new a partial page and save it as `_form.html.haml`.      

Then, inside of our `app/views/articles/_form.html.haml`
```haml
= simple_form_for @article do |f|
	= f.input :title
	= f.input :content
	= f.submit
```

And then go back to `app/views/articles/new.html.haml`
```haml
%h1 New Article

= render 'form'

= link_to 'Back', root_path
```

Back in our `app/controllers/articles_controller.rb`, let's define our show action.
```ruby
class ArticlesController < ApplicationController
	before_action :find_article, only: [:show]

	def index
	end

	def show
	end

	def new
		@article = Article.new
	end

	def create
		@article = Article.new(article_params)
		if @article.save
			redirect_to @article
		else
			render 'new'
		end
	end

	private

	def find_article
		@article = Article.find(params[:id])
	end

	def article_params
		params.require(:article).permit(:title, :content)
	end
end
```

Let's go ahead and create a new file under `app/views/articles` and save it as `show.html.haml`.
```haml
%h1= @article.title
%p= @article.content

= link_to "Back", root_path
```
![image](https://github.com/TimingJL/wiki/blob/master/pic/test_article.jpeg)


Next, let's add the ability to create a new article from the homepage.        
In `app/views/articles/index.html.haml`
```haml
%h1 This is the articles#index placeholder

= link_to "New Article", new_article_path
```

### List Out All Of The Article On The Homepage
Next, we wanna list out all of the article on the root of the application.       
In `app/controllers/articles_controller.rb`
```ruby
def index
	@articles = Article.all.order("created_at DESC")
end
```

In `app/views/articles/index.html.haml`, we add the title, created date, and preview for our article.
```haml
- @articles.each do |article|
	%h2= article.title
	%p
		Publish at
		= article.created_at.strftime('%b %d, %Y')
	%p
		= truncate(article.content, length: 200)

= link_to "New Article", new_article_path
```
![image](https://github.com/TimingJL/wiki/blob/master/pic/loop_articles.jpeg)


# User
The next thing I wanna do is add devise gems to our user.
```console
$ rails generate devise:install


===============================================================================

Some setup you must do manually if you haven't yet:

  1. Ensure you have defined default url options in your environments files. Here
     is an example of default_url_options appropriate for a development environment
     in config/environments/development.rb:

       config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }

     In production, :host should be set to the actual host of your application.

  2. Ensure you have defined root_url to *something* in your config/routes.rb.
     For example:

       root to: "home#index"

  3. Ensure you have flash messages in app/views/layouts/application.html.erb.
     For example:

       <p class="notice"><%= notice %></p>
       <p class="alert"><%= alert %></p>

  4. You can copy Devise views (for customization) to your app by running:

       rails g devise:views

===============================================================================
```


Let's add flash messages in `app/views/layouts/application.html.erb`.
```html

	<!DOCTYPE html>
	<html>
	  <head>
	    <title>Wiki</title>
	    <%= csrf_meta_tags %>

	    <%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track': 'reload' %>
	    <%= javascript_include_tag 'application', 'data-turbolinks-track': 'reload' %>
	  </head>

	  <body>

		<p class="notice"><%= notice %></p>
		<p class="alert"><%= alert %></p>
		
	    <%= yield %>
	  </body>
	</html>
```

And we can convert Html to Haml quickly by using this tool:         
http://htmltohaml.com/      

So first, we rename `application.html.erb` to `application.html.haml` under `app/views/layouts`. And then convert the Html to Haml.
```haml
!!!
%html
  %head
    %meta{:content => "text/html; charset=UTF-8", "http-equiv" => "Content-Type"}/
    %title Wiki
    = csrf_meta_tags
    = stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track': 'reload'
    = javascript_include_tag 'application', 'data-turbolinks-track': 'reload'
  %body
    %p.notice= notice
    %p.alert= alert
    = yield
```


And the we generate devise views by running:
```console
$ rails g devise:views
```



The next thing we want to do is add a model for our users.
```console
$ rails g devise User
$ rake db:migrate
```

Let's restart our server and go to `http://localhost:3000/users/sign_up`, we'll see:
![image](https://github.com/TimingJL/wiki/blob/master/pic/basic_signup.jpeg)

Now, we have the ability to create a new user.



### Add Association
SO next what we want to do is add association between the article model and the user model.        

In `app/models/article.rb`
```ruby
class Article < ApplicationRecord
	belongs_to :user
end
```

And then in `app/models/user.rb`
```ruby
class User < ApplicationRecord
  # Include default devise modules. Others available are:
  # :confirmable, :lockable, :timeoutable and :omniauthable
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :trackable, :validatable
  has_many :articles
end
```

### Create a Migration to Add User ID
The next thing we need to do is create a migration to add a `user_id` to the articles table in our database.
```console
$ rails g migration add_user_id_to_articles user_id:integer:index
$ rake db:migrate
```


### Build Out A New Article
Next thing we want to do is build out a new article from the current user.        
In `app/controllers/articles_controller.rb`, we need to tweak the `new` action and the `create` action a bit.
```ruby
def new
	@article = current_user.articles.build
end

def create
	@article = current_user.articles.build(article_params)
	if @article.save
		redirect_to @article
	else
		render 'new'
	end
end
```

Let's test it in rails console:
```console
$ rails c

> @article = Article.last
```

And you can see the `user_id` in the very end is `nil`.
```
  Article Load (0.7ms)  SELECT  "articles".* FROM "articles" ORDER BY "articles"."id" DESC LIMIT ?  [["LIMIT", 1]]          
=> #<Article id: 4, title: "HTML", content: "HyperText Markup Language, commonly abbreviated as...",           
created_at: "2016-08-10 10:39:55", updated_at: "2016-08-10 10:39:55", user_id: nil>
```

Next, let's try creating a new article which has the `user_id: 1`.
Let's go to `http://localhost:3000/articles/new` to create a new article.        
Then back to our console:
```console
$ rails c

> @article = Article.last
```

You can see the last article we just create has `user_id` of 1. That is working correctly.
```
  Article Load (0.2ms)  SELECT  "articles".* FROM "articles" ORDER BY "articles"."id" DESC LIMIT ?  [["LIMIT", 1]]         
=> #<Article id: 5, title: "Haml", content: "Haml (HTML Abstraction Markup Language) is a templ...",          
created_at: "2016-08-10 15:40:27", updated_at: "2016-08-10 15:40:27", user_id: 1>
```

Then we assign `user_id` to the previous articles to avoid errors (or you can delete these articles which `user_id` is `nil`).
```console
> @article = Article.first
> @article.user_id = 1
> @article.save

> @article = Article.find(2)
> @article.user_id = 1
> @article.save
...
...

```







To be continued...