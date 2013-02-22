<h1>How to do tagging in Rails with gem ‘Acts_as_taggable_on’?</h1>

Tagging is a way to give a keyword and search by that particular keyword. In Rails “acts-as-taggable-on” is one of the popular gem for tagging. With acts_as_taggable_on, you can tag a single model on several contexts, such as skills, interests, and awards. 

Below is an example to describe how to implement it in a Rails application:

<h6>Step#1</h6>

Add the gem to your Gemfile:

gem ‘acts-as-taggable-on'
Then run the bundle

bundle install

<h6>Step#2</h6>

Run the generator command

rails g acts_as_taggable_on:migration
Run the migration file

rake db:migrate
This migration will create two tables in the database: tags and taggings.

<h6>Step#3</h6>

Now you have to modify your model where you want to implement tagging.
For example, we like to implement the tagging on Post model.
Add the following codes to the “Post” model to have acts_as_taggable and also add a “tag_list” field to the attr_accessible list.

attr_accessible :content, :name, :tag_list
acts_as_taggable

<h6>Step#4</h6>

Add a new field to your post form.

<%= f.label :tag_list, "Tags (separated by commas)" %>
<%= f.text_field :tag_list %>

<h6>Step#5</h6>

Add the following codes, if you want to show the tags for the tagged posts in the listing page:

<% @posts.each do |post| %>
<h2><%= link_to post.name, post %></h2>
<%= simple_format post.content %>
<p>
Tags: <%= raw post.tag_list.map { |t| link_to t, tag_path(t) }.join(', ') %>
</p>
<% end %>

<h6>Step#6</h6>

Modify your route file to get the tags

get 'tags/:tag', to: 'posts#index', as: :tag
Step#7

Modify your controller:

  class PostsController < ApplicationController
  
      def index
  
      if params[:tag]
        @posts = Post.tagged_with(params[:tag])
      else
        @posts = Post.all
      end
    end
    
  end
  
Voila! You have done it.
