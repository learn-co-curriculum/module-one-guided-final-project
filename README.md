# module-one-final-project-directions

Congratulations, you're at the end of module one! You've worked crazy hard to get here and have learned a ton.

For your final project, we'll be building a Command Line database application.

### Goals for the Command Line CRUD App

1. Build at least three models with corresponding tables, including a join table
2. Access a Sqlite3 Database using ActiveRecord
3. Build out a CLI to give your user full CRUD ability for at least one of your resources. For example, build out a command line To-Do list. A user should be able to create a new to-do, see all todos, update a todo item, and delete a todo. Todos can be grouped into categories, so that a to-do has many categories and categories have many to-dos.
4. Use good OO design patterns. You should have separate models for your runner and CLI interface.

## Instructions
1. Fork and clone the module one final project. The person who forked the lab should share the link with their teammate(s) to clone. As you work, be sure to create a flow of creating a branch, committing and pushing it up to master, merging, and having teammates pull down the new master.
2. Before you start building, take a look at the files you have available in this repo. In the main directory, you've got a gemfile that gives you access to activerecord, pry, rake, and sqlite3 througout your app. Remember to bundle install! In the bin directory, you've got a run.rb file that you can run from the command line with 'ruby bin/run.rb.' In config, you've got your database set up with activerecord, as well as all of your models from the lib file made available to your database. In the lib directory, you'll be building all your models. 
3. Your first goal should be to decide on your models and determine the relationships between them. You'll need one many-to-many relationship. 
  Here are some ideas: 
    - Train Line, Station, Station Lines: A line has many stations and a station has many lines, station_lines belongs to line and station
    - Movie, Actor, Movie Actors: A movie has many actors and an actor has many movies, movie_actors belongs to line and station
    -Tweet, Topic, Tweet topics: A Tweet has many topics and a topic has many tweets, tweet_topic belongs to tweet and topic
    
    Whiteboard out your ideas and think about what columns you'll want in the corresponding tables, including foreign keys. Where are foreign keys stored in a many-to-many relationship? Get your data modeling approved by an instructor before moving on to the next step. 
4. Make a new file for each model in your lib folder. What's the naming convention for a model filename? Check out previous labs for a reminder. Remember that activerecord gem from our gemfile? Make sure that every model with a corresponding database table inherits from activerecord. 
5. Be sure to include the relationships between your models. The <a href="http://guides.rubyonrails.org/association_basics.html">activerecord documentation</a> is a great source if you get stuck! Check out the has_many :through section when setting up your many-to-many relationship.
6. Create your database and migrations in the terminal (keeping in mind that you have Rake available to you! Run rake -T in your terminal for a refresher.) What are the naming conventions for migration files and table names?
7. Now is a great time top open up your console in the terminal and make sure everything's working properly. Your database is empty at this point, so start by creating a new row in your table. For the train example, we'd do something like this:
```
fulton = Station.new(name: "Fulton")
```

  

