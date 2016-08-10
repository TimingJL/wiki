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

First, we add some rubygems that we're going to use.
In `Gemfile`
```
gem 'therubyracer'
gem 'devise'
gem 'simple_form', github: 'kesha-antonov/simple_form', branch: 'rails-5-0'
gem 'haml', '~> 4.0.5'
gem 'bootstrap-sass', '~> 3.2.0.2'
```


To be continued...