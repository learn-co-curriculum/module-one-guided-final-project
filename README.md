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
fulton = Station.create(name: "Fulton")
```
You should see the station inserted into your database. Cool! Now let's test our relationships:
```
a = Line.create(name: "A")
fulton.lines << a
```
Woah! What did we just do there? The first part is simple: we added the a line to our line table. Then, we accessed our station, Fulton Station, and accessed its array of lines. (Because a station has many lines, right?) Finally, we pushed the A line into Fulton's lines. Amazing!! High fives all around.

8. At this point, we could continue adding items to our database through the console, but let's be real. There are 425 train stations in New York--entering them individually would take forever!! There must be a better way... Enter the seeds file. What is a seed file? It's a file, located in the db folder, where you create new instances of your classes and save them to your database. There are several ways this could happen. You could iterate over as csv file, for example, pulling out relevant data, and creating a new row in your database for every row in the file. 

For now, let's just manually create some objects, and set up the relationships between them. You can do it in exactly the same way we did in the console. 

You'll want to make sure you have enough data to play around with once you get your command line interface up and running. Five to ten instances of each model, as well as the corresponding relationships should be enough. You can always add more later. Once your file is ready, run rake -T to see which rake task you can use to seed your database.

9. Okay, so we've got our databases; we've got our models; and we've got our relationships set up between them. Now, what do we do with all this stuff? We don't want our users to have to use the console every time they want to see which train lines go through fulton station, so let's create a command line interface! 

First things first, let's open up the runfile and create a method that greets our app user. Then, let's call the method.
```
def greet
  puts 'Welcome to TrainFinder, the command line solution to for your MTA train-finding needs!"
end

greet
```
Now, let's run the file from the terminal:
```
ruby bin/run.rb
```
We should see our welcome message printed. Rad!! But wait--why are we defining this method in the runfile? Isn't that going to get messy? Answer: yes.

10. Instead, let's create a Command Line Interface model in our lib directory. This model won't have a corresponding table, it's just going to be a place for us to write methods relating to the interface of our app. Now, let's move the greet method definition into the Command Line Interface model.

Now, our bin/run.rb should create a new instance of our Command Line Interface model and call the instance method, greet.

```
new_cli = CommandLineInterface.new
new_cli.greet
```
Run ruby bin/run.rb to make sure everything works!

11. Alright, we've greeted our user, but so far we haven't given them any information that we worked so hard to store in our database. Let's give them some of our valuable data! First things first: how should we decide what to show our users? It would probably be overwhelming if we printed out every line for every station in New York city, so let's ask the user which station they'd like to see lines for.

Create a new method in the Command Line Interface model:

```
def gets_user_input
  puts "We can help you find which train lines are available at NYC subway stations."
  puts "Enter a subway station to get started:"
  gets.chomp
end
```

Now, call it from the run file and run your app to make sure everything works!

12. So, we've gotten an input from our user, but what do we do with the it?

First let's think big picture: our goal is to take the user's input--a string of a station name--and use that name to find a station in our database. Then we want to grab all of that station's lines and output the lines to our user. 

Now let's think about how we'll code out this process: first, we want a method that uses the station string to query our database, right? Know any activerecord methods we could use to find a station by its name?

```
def find_station(station)
  Station.find_by(name: station)
end
```
Okay, so now we can find our station, but how do we hand off the user's input to the find station method?

We could do something like this in our run file...
```
new_cli = CommandLineInterface.new
new_cli.greet
input = new_cli.gets_user_input
new_cli.find_station(input)
```
...but let's not!

Instead, let's move our individual method calls from the run file to a runner method in our Command Line Interface model. 

```

def run
  greet
  input = gets_user_input
  find_station(input)  
end

```
And let's change our run file to just call the run method.

13. So, we can greet our user, grab their input, and use that input to find a station. We're on the home stretch, but there are a few things left to do. First, we need to find the lines associated with the station, and then we need to output the lines to the user.

First, let's make a method to access the station lines and add it to our run file:

```
def find_lines(station)
  station.lines
end

```
The find lines method takes in an instance of station and returns that station's lines. How can we pass the station that we found in find_station as the argument in find_lines? How about in our run file! Below we've set the return value of find_station to a variable, station. Now use that vairable to find the lines.

```
def run
  greet
  input = gets_user_input
  station = find_station(input)  
end
```
