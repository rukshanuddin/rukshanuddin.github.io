---
layout: post
title:      "Sinatra Tips & Tricks!"
date:       2020-01-30 04:30:41 +0000
permalink:  sinatra_tips_and_tricks
---

## Somewhat of a walkthrough...

Sinatra, surprisingly enough was the toughest module I've had to face, so I decided to put together a list of handy tips and tricks to help the next student who faces difficulty in doing their project.

### Corneal

The easiest way to get a Sinatra project going, where you can utilize CRUD is to use Corneal. Corneal is a gem that will help create models, views and controllers for your project so you can utilize MVC. So we begin with installing Corneal with 

`gem install corneal`

### Create a project

To create your project, you first need to prepare a folder for your repo and then type in:

`corneal new APP-NAME`

APP-NAME being the name of your Sinatra app, I called mine Student-Portfolio

### Models

Now we figure out the M of the MVC. M is for Model and to create our models we use

`corneal model NAME`

NAME being the models you will use for your project. I used User and Project for my models. So I entered :

`corneal model User`

and

`corneal model Project`

this will create the models and migrations that our project will use.

### Migrations

Once my migrations were created by the Corneal gem all I had to do was enter my fields into it. I used

class CreateUsers < ActiveRecord::Migration

  def change

   create_table :users do |t|

      t.string :firstname

      t.string :lastname

      t.string :username

      t.string :password_digest


      t.timestamps null: false

    end

  end

end

for my Users table and:

class CreateProjects < ActiveRecord::Migration

  def change
	
    create_table :projects do |t|
		
      t.string :title
			
      t.string :module
			
      t.string :description
			
      t.string :github
			
      t.integer :user_id
			
      t.timestamps null: false
			
    end
		
  end
	
end


The cool thing is that ActiveRecord and Corneal takes care of most of the work and all I had to do was add the attributes and their types that I wanted my models to have in the migrations file.

### Models

The only thing I had to do next was edit the models to include some features. I included `has_secure_password` to my User model to allow bcrypt and :password_digest to work and `has_many  :projects` to show the relationship that Users had their own projects. I added `belongs_to :user` to the Project model to show that the User owned the Project.

### Views & Controller

Corneal will create many of the views required by a model controller automatically with

`corneal controller NAME`

NAME being the model that we are building views and controllers for. I once again used Project and User and ended up with all my views and controllers for the models realy to edit.

Now although Corneal will create generic views and controller routes, it is up to the coder to really modify wherever they see fit. I for instance did not use all the views created and I also used views not auto-generated. It all depends on the needs of the application. I ended up using all the generated project views, but didn't use the show or edit views created by the Corneal gem. I did however create two other views in my view/users folder.

### Layout

I used the layout.erb file to create some nav bars for my app, not to difficult, just incorporating basic Ruby to check if a user was logged in to display one of 2 nav bars.






