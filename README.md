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




To be continued...