## Welcome to CS350

## Contents
### [Video 0: Introduction to Vagrant](#video-0-introduction-to-vagrant-1)
###[Video 00  Google Cloud Intro](#video-00-google-cloud-intro)
### [Video 1: Getting Started with Cloud9](#video-1-getting-started-with-cloud9-1)
### [Video 2: Getting Started with Flask](#video-2-getting-started-with-flask-1)
### [Video 3: Flask Template Basics](#video-3-flask-template-basics-1)
### [Video 4: Flask Templates - Variables, Conditionals, and Loops](#video-4-flask-templates---variables-conditionals-and-loops-1)
### [Video 5: PostgreSQL on Cloud9](#video-5-postgresql-on-cloud9-1)
### [Video 6: PostgreSQL and the Principle of Least Privilege](#video-6-postgresql-and-the-principle-of-least-privilege-1)
### [Video 7: Flask and HTML forms](#video-7-flask-and-html-forms-1)
### [Video 8: PostgreSQL and Flask: Using the psycopg2 adapter](#video-8-postgresql-and-flask-using-the-psycopg2-adapter-1)
### [Video 9: hints, common errors, and bad practices](#video-9-hints-common-errors-and-bad-practices-1)
### [Video 10: Session Variables and Hashing Passwords](#video-10-session-variables-and-hashing-passwords-1)
### [Video 11: Learning the rudiments of sessions and passwords](#video-11-learning-the-rudiments-of-sessions-and-passwords-1)
### [Video 12: SocketIO Part 1: AngularJS and flask-socketio](#video-12-socketio-part-1-angularjs-and-flask-socketio-1)

# Video 0: Introduction to Vagrant
The process:

1. develop code on whatever development environment you want
2. push your code to a git repository (for ex., gitlab)
3. pull the code onto your production machine (for example, a VM on Google Cloud)
4. run your code on your production machine

As 1 states, you can develop on whatever environment you want. This video covers one of them, vagrant, the next video covers Cloud9.

### Vagrant
First, we installed Vagrant. To get a virtual machine running:

**Step 1**. We created a directory and changed into it. So on my Mac:

	mkdir cs350
	cd cs350

**Step 2**: Decide on an OS and create the VagrantFile. Here I am using ubuntu yakkety64.

	vagrant init ubuntu/yakkety64
	
**Step 3**: I edit the config file to uncomment (and change the guest port to 8080)

	config.vm.network "forwarded_port", guest: 8080, host: 8080

and I save that file.

**Step 4**  Start the virtual machine

	vagrant up
	
This takes a bit of time the first time you do it.  If you want to stop the machine:

	vagrant halt
	
and if you want to get rid of the machine

	vagrant destroy
	
**Step 5**  ssh into the machine

	vagrant ssh
	
Now I am in the machine just like ssh-ing into other machines you are familiar with. This one you have total control over.

To exit ssh:

	exit
	
**Step 6**  the shared directory
the `/vagrant` directory is a shared directory. It shares whatever folder you were in when you typed `vagrant up`

	cd /vagrant
	ls
	
So I can edit files using my favorite Mac editor (Sublime) and execute those files on my Ubuntu VM.

**Step 7** Installing software
So I ssh into the machine. Now I want to install the software I will use:
First I will install Flask:

	pip install flask markdown
	
or

	easy_install flask markdown
	
How to use Flask is explained in another video.


	
# Video 00 Google Cloud Intro

Goal - one look at deploying an app.

1. how to get started with a virtual machine in google cloud.
2. how to install what we need for the class. - or at least a few examples.
3. ways of getting our web app started.



# a lot of exciting technology
* docker containers
* google app engine
* various google database services

a virtual machine is perhaps not as exciting but will provide a good foundation.

## these notes don't cover actually creating the vm on google. 

## flask prep
Now install some python stuff I need

     sudo apt-get install python-setuptools
     sudo easy_install flask markdown
     



## POSTGRES

	sudo apt-get install postgresql
	sudo sudo -u postgres psql
	\password kirbyk9
	



## ways of starting flask app

	sudo python server.py
	
option 2 is to use gunicorn

	sudo gunicorn -w 2 -b :80 server:app
	
option 3 - add supervisor

following [these instructions](https://www.digitalocean.com/community/tutorials/how-to-install-and-manage-supervisor-on-ubuntu-and-debian-vps)

     apt-get install supervisor
     

my config file

	[program:uni_app]
	command=/usr/local/bin/gunicorn -b :8080 server:app
	autostart=true
	autorestart=true
	directory=/vagrant/rcClub
	stderr_logfile=/var/log/long.err.log
	stdout_logfile=/var/log/long.outy.log
	
stick that in `/etc/supervisor/conf.d`

then	

	sudo supervisorctl reread
	sudo supervisorctl update
	sudo supervisorctl restart uni_app
	





# Video 1: Getting Started with Cloud9
No summary provided.


## hack to get rid of Django warning

All this from the command line (and assuming you are in the workspace directory:

     sudo pip install virtualenv
     virtualenv venv
     source venv/bin/activate
     pip install django
     
Then close the terminal

Open up the preferences.  Scroll to the language support > Python section and add the following to the end of the pythonpath

     :/home/ubuntu/workspace/venv/lib/python2.7/site-packages
 
 
------

# Video 2: Getting Started with Flask!

## Web Application Framework
* dynamic websites
* web apps

## Flask: micro web application framework
* doesn't have a database abstraction layer
* doesn't force us to use a particular tool or library
* lean

## Happy Coding!

### Installing Flask and Markdown

     sudo easy_install flask markdown
     
### The most basic server code:

     import os
     from flask import Flask
     app = Flask(__name__)
    
     @app.route('/')
     def helloWorld():
         return "Hello World"
    
     # start the server
     if __name__ == '__main__':
         app.run(host=os.getenv('IP', '0.0.0.0'), port=int(os.getenv('PORT', 8080)), debug=True)
     
    
     
------

# Video 3: Flask Template Basics

1. cloning a git repository [https://github.com/zacharski/templateIntroV2.git](https://github.com/zacharski/templateIntroV2.git)
2. finding free html templates
3. Flaskifying a template
4. add pages

## Next time:
* dynamic content!
* 80% of what you need to know about Flask for this course

------

# Video 4: Flask Templates - Variables, Conditionals, and Loops
 
## simple variables
On the html side, the variable name is bounded by double braces:

     This week's flight range manager is {{manager}}

on the Python side we pass that variable to render_template

     theManager = "Ann Mulkern"
     return render_template('index.html, manager=theManager)
     
Note that the name to the left of the equals (=) must match the variable name in the html file.

## passing Python dictionaries to the template

on the Python side:

     project = {'date': '15 January', 'time': '2pm', 
                'company': 'FliteTest', 'model': 'Easy Flyer'}
      return render_template('index.html', proj=project)
      
and on the html template side we use a dot notation:

     The next build your own workshop is {{project.data}} at
     {{project.time}} where we will be building 
     {{project.company}}'s {{project.model}}!
     
## format of conditional statements

Python side:

     isFlying = True
     return render_template('index.html', flyingP= isFlying)
     
and on the HTML template side:

     <h1>Welcome to the UMW R/C Club</h1>
     {% if flyingP %}
     <p><b>We are meeting this Wednesday</b></p>
     {% else %}
     <p><b>We are not meeting this Wednesday</b></p>
     {% endif %}
     
We can use comparison operators such as

     {% if windSpeed > 10 %}
     <p>Blah, blah</p>
     {% endif %
     
## looping through a list

Python side:

     videos = [{'title': 'R/C with Dogs', 
                'vidLink': 'y0U_25vKpDs', 
                'description': 'Hoffy is a puppy at heart with the body of full grown dog. In other words, he\'s kind of insane.'}, 
		{'title': 'Quad Copter Shootout', 
		 'vidLink': 'mNLqpBFwOXY', 
		 'description': 'The guys comparing their experiences flying the different quads'}, 
		{'title': 'Argonay Miniquad Racecourse', 
		'vidLink': 'JpVY09Ce5C8', 
		'Description': 'We had the honor of flying with the Airgonay RC Club in their breathtaking miniquad racecourse. '}, 
		{'title': "Microwave Plane", 
		'vidLink': 'vF5UMQl9NUQ', 
		'description': "Attempting to pop popcorn in a microwave in the air!"}]
		
    return render_template('index.html', posts=video)


and on the HTML template side:

        {% for post in posts %}
        <h2>{{post.title}}</h2>
        <iframe width="620" height="480"
           src="https://youtube.com/embed/{{post.vidLink}}"></iframe>
        <p>{{post.description}}</p>
        {% endfor %}

a simpler example might be

Python:

     members = ['Ann', 'Ben', 'Clara', 'Dora']
     return render_template('index.html', crew=members)
     
HTML Template
   
     <h3>Current members include</h3>

     {% for person in crew %}
     <p>{{person}}</p>
     {% endfor %} 
     
------


# Video 5: PostgreSQL on Cloud9

### starting the server

          sudo service postgresql start
     
### creating a password

     sudo sudo -u postgres psql
     # setting the password
     \password postgres
     # then quit
     \q
     
### logging into the client with your new password

     psql -U postgres -h localhost
     
Or, let's say you want to login as user `swift` to the `pets` database

     psql -U swift -d pets -h localhost
     
### useful commands

| command | description |
|:---:|:---|
|`\q` | exit the postgres client |
|`\l` or `\list` | list all the databases |
| `\c pets` | change to the pets database |
| `\d` or `\dt` | show a list of all the tables in the current database |
| `\d dogs` | describe the format of the table `dogs` |
| `\d+ dogs` | describe  *in more detail* the format of the table `dogs` |
| `\i charts.sql` | import sql commands from the file `charts.sql` |
| `DROP DATABASE charts` | delete the database `charts` |
| `SELECT * FROM topSongs` | show all the data in the `topSongs` table



# Video 6: PostgreSQL and the Principle of Least Privilege

### Principle of Least Privilege
A user (or process) should have the lowest level of privilege required in order to perform his/her assigned task.

Microsoft says:

> running in standard user mode gives customers increased protection against inadvertent system-level damage caused by "shatter attacks" and malware such as root kits, spyware, and undetectable viruses.


Similar to:

* compartmentalization (need to know)
* encapsulation (restrict access to object's components)

which leads to better security and better stability


### True for database servers
1. Your Flask app needs to login to the database server.
2. If your Flask app uses the root account that means the root username and password are stored in your code.
3. If you screw up: 
    1. your code itself can destroy all the databases on the server.
    2.  a malicious attack on your code could compromise all the databases on the server and possibly compromise the server itself.
4.  The solution is to create an account with the least privilege required to do the task.

### Incentive
Any Flask PostgreSQL code that uses the root postgres account will get 0xp.

## creating user accounts
| command | description |
|:---:|:---|
| `CREATE ROLE petman;` | will create a user named `petman` |
| `CREATE ROLE swift;` with login | will create a user named `swift` with login privileges (probably what you want) |
| `DROP ROLE swift;` | delete the user named `swift` |
|`\du` | list all the users |
|  `ALTER ROLE petman with login;` | give login privileges to `petman` |
| `\password swift` | set a password for the user `swift` (you will be prompted) |
| `GRANT ALL ON topsongs TO SWIFT;` | grant all permissions to the table `topsongs` to the user `swift`  
| `GRANT SELECT ON topsongs TO petman;` | only grant `select` privileges to the `topsongs` table to `petman`. (For all options see [https://www.postgresql.org/docs/9.0/static/sql-grant.html](https://www.postgresql.org/docs/9.0/static/sql-grant.html)) |
| `REVOKE ALL ON topsongs FROM petman;` | revoke all permissions on the table `topsongs` from user `petman` |
| `\z` | show what permissions users have on the current database |






creating a user account (or role) 

     CREATE ROLE <username>;
     #for example
     CREATE ROLE swift;  	
     
# Video 7: Flask and HTML forms!

### Getting input from users!

#### User input is bounded by the `form` tag

     <form method="POST" action='/simple>
     ...
     </form>

There are several arguments we are using in the opening form tag:

### method
     <form method="POST" action='/simple>
method is how the information is sent from the client (the browser) to the server. It can be:

1. GET
     * limited to 256 characters
     * use if the processing of a form is idempotent (no side effects--doesn't change anything on the server). For example, a search option has no side effects)
2. POST
     * unlimited data, more secure
     * use of there are no side effects.
     
## action

     <form method="POST" action='/simple>
action is what to execute on the server side to process the info in the form. So

     <form method="POST" action="/simple.php">
     when the user presses the submit button, the php script
     simple.php is executed on the server.
     
    <form method="POST" action='/register'>
    looks for the /register decorator in Flask
    
## a simple submit button example:

HTML side:

     <h2>Don't Press that Button</h2>
     
     <form method="POST" action="/registration2">     
     <input type="submit" value="Press Me">
     </form>

when the user presses the `Press Me` button the Python code associated with the `registration2` decorator will be executed. So we need to add that code:

     @app.route('/registration2', methods=['POST'])
     def reply():
         return render_template('r2.html')
         
## text input

    <input type="text" name="plane" />

the value of the `name` attribute is the name we want to give this variable.

Let's add this to the form, and to make things look nice, let's put it in a table:

     <h2>Register Your Plane</h2>
     
     <form method="POST" action="/registration2">     
     <table>
       <tr><td>Model Plane</td>
           <td><input type="text" name="plane" /></td></tr>
     </table>     
     <input type="submit" value="Press Me">
     </form>


### Using the value in our Python code
First we need to import another module, the `request` module:

    from flask import Flask, render_template, request 
    ...
    
Now we can access the value of that input field by using `request.form['plane']`:



     @app.route('/registration2', method=['POST'])
     def reply():
         theplane = request.form['plane']
         ...
         return render_template('r2.html')

### checking whether we got to the current code by a user pressing the submit button:

     if request.method== "POST":
         do something
     else # we got here some other way




# Video 8: PostgreSQL and Flask: Using the psycopg2 adapter

### 7 basic steps
1. import the psycopg2 library modules
2. connect with `psycopg2.connect`
3. create a cursor object (the object that interacts with the db)
4. assemble the query string 
5. execute the query string with `cur.execute()``
5. fetch the results
6. if inserting make sure to use `conn.commit()`


### installing the psycopg2 module

#### first, update the package list

     sudo apt-get update
     
#### then install the adapter     

     sudo apt-get install python-psycopg2
     
For this demo we will be using the repository:

     git clone https://github.com/zacharski/AlienAbductions2.git
     
### the seven steps

#### step 1: import libraries
  
     import psycopg2
     import psycopg2.extras
     
#### step 2: connect with psycopg2.connect
for this I like to create a function

     def connectToDB():
         connectionStr = 'dbname=music user=music123 password=2Vk0H host=localhost'
         try:
              return psycopg2.connect(connectionStr)
         except:
             print("Can't connect to database")
             
Now I create a connection by calling this function:

     conn = connectToDB()
     
#### create a cursor object

     cur = conn.cursor()
     
or if we want to have the results be a list of Python dictionaries:

     cur = conn.cursor(cursor_factory=psycopg2.extras.DictCursor)
     
#### steps 4 & 5: assemble and execute the query:

     cur.execute("SELECT artist, name FROM albums")
          
#### step 6: fetch the results:

     results = cur.fetchall()
     

#### step 7: use conn.commit() if inserting

     try:
        cur.execute('INSERT INTO albums VALUES...'))
     except:
        conn.rollback()
     conn.commit()
----    

# Video 9: hints, common errors, and bad practices

## tuples
Python has 4 array-like data structures (including an associative array)

| type | example |
|:--|:--|
| list (the most common) | `aList = ['Ann', 'Ben', 'Clara'] `|
| tuple | `aTuple = ('Ann', 'Ben', 'Clara')` |
| associative array | `aDict = {'Ann': 21, ''Ben': 19, 'Clara': 18}`|
| array | `aArray = array.array('l', [1, 2, 3])`|

Arrays, while more efficient than lists for numeric values,  are rarely used. If your goal is to do fast processing on numeric arrays it is much better to use the `numpy` library.

The differences between lists and tuples are that 

* lists are mutable and tuples are not  meaning
     * you can't add items to a tuple
     * you can't remove items from a tuple
* tuples are faster than lists

### tuple oddities
One major use of tuples is in string replacement:

     >>> areFriends = ('Ann', 'Clara')
     >>> areFriends
     ('Ann', 'Clara')
     >>> print('%s and %s are friends' % areFriends)
     Ann and Clara are friends

As you can see, we created a tuple `areFriends` and checked the value, which indeed is a tuple. And then we use the tuple in the print statement to do string replacement.

You cannot do the same with lists:
     
     afa
     >>> friends = ['Ann', 'Clara']
     >>> print('%s and %s are friends' % friends)
    Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    TypeError: not enough arguments for format string

So we use tuples for this string formatting. 

Again, we create a tuple like this:


     >>> areFriends = ('Ann', 'Clara')

And you would think that this could create a one element tuple by doing

     >>> country = ('Canada')

But when we check the value of the variable country we see that it is the string 'Canada' and not a tuple:

     >>> country
     'Canada'
     
If we actually want a one element tuple (and as you will shortly see, you will) you create it by adding a comma:

     >>> country = ('Canada', )
     >>> country
     ('Canada',)

## RULE: always use the cursor object to do string substitutions:

Do this:

     cur.execute("select * from x where y = %s and z = %s" , (var1, var2))

and not

    cur.execute("select * from x where y = %s and z = %s"  % (var1, var2))

The difference is between a comma and a semi-colon. The videos explain why we should do this. **This is super important**

Another good option is

    query = cur.mogrify("select * from x where y = %s and z = %s"  % (var1, var2))
    # for debugging purposes 
    # print the query
    print(query)
    # now execute
    try: 
        cur.execute(query)
    except:
        print('Error executing: %s' % query)

# Video 10: Session Variables and Hashing Passwords
HTTP is a stateless protocol. It has no built-in way to maintain state between 2 transactions.

**Global variables do not work (at least on their own) to store session data**
## Session Variables

1. Session Variables are stored on the server.
2. They persist across multiple pages
3. Unlike cookies, they have no expiration date. They die when the session ends.
4. A session can end when you close the browser. Cookies can persist even when you close your browser.

## Using session variables

### 1. starting a session

     from flask import Flask, session
     import os
     app.secret_key = os.urandom(24).encode('hex')
     
The secret key is to sign cookies that are used by the Session (you don't need to handle these cookies - it is done for you)

### 2. setting and accessing session variables

To set a session variable
  
      session['username'] = 'Ann'
      session['plane'] = ['Impulse RC Alien']
      session['zip'] = 88005
      
Using a session variable value:

     print(session['username'])
     query = cur.mogrify('SELECT * FROM users WHERE name= %s', session['username'])
     
### 3. delete a session variable

     session.pop('name', None)
     
## Hashing Passwords

### NEVER EVER STORE PASSWORDS IN THE CLEAR
*in the clear* means human readable.

*example*

First, we create a users table

     create users (username text, password text);

    
We shouldn't do this:

     insert into users values ('ann', 'changeme'), ('Ben', 'w0r1dP34c3');
     
Because anyone who gains access to the database can see everyone's password:

     SELECT * FROM users;
     
username | password 
----------+----------
ann | changeme
ben| w0r1dP34c3

(2 rows)

Maybe you think, *who cares if someone gets the passwords for the RC club -- its not that important.*  But Ben might think his password is great and use it, not only for the UMW RC Club, but for Capital One and other important sites. 

## Steps to hashing passwords

### 1. install the package (you only need to do this once per postgresql server)
(so, in cloud9, you only need to do this once per virtual machine you create)

     sudo apt-get install postgresql-contrib-9.3
     
### 2. enable hashing within the Database
You need to do this for each database in which you wish to hash passwords. For example, If both the databases , `pets` and `music` have a table that stores passwords you need to do this for both databases.  
**NOTE:** If you are creating an SQL file that builds your database, you can put this command in that file

The command is:

     CREATE EXTENSION pgcrypto;

So, for example, here I log into the postgres client, psql, change to the `world` database, and execute the command.

     mitsugo:~/workspace $ psql -U postgres -h localhost
     Password for user postgres: 
     ...

     postgres=# \c world
     You are now connected to database "world" as user "postgres".
     world=# CREATE EXTENSION pgcrypto; 
     CREATE EXTENSION
### 3. hashing passwords when inserting into the table

If I am inserting a person with username `ann` and password `changeme`:

     INSERT INTO users VALUES ('ann', crypt('changeme', gen_salt('bf')));

* For an 8 character a-z password it would take 246 years to crack via brute force.
* For an 8 character a-z, A-Z, 0-9 password it would take 251,322 years to crack.

Here is what the password looks like within the table:

     world=# SELECT * FROM users;
 username |                           password                           
----------+--------------------------------------------------------------
 ann      | $2a$06$AFHesYQKTEMU.YSBJlK0i.ZOsRyGBvgZ1aTI6cV1judtBa9plnrge
(1 row)


### 4. checking if user entered correct password
let's say a person types in `ann` with the correct password and we check it via

     world=# SELECT * FROM users WHERE username = 'ann' and 
     password = crypt('changeme', password);
 username |                           password                           
----------+--------------------------------------------------------------
 ann      | $2a$06$AFHesYQKTEMU.YSBJlK0i.ZOsRyGBvgZ1aTI6cV1judtBa9plnrge
(1 row)

so we get a result back. Now here is an example of a person typing in the incorrect password:

     world=# SELECT * FROM users WHERE username = 'ann' and 
     password = crypt('123456', password);

 username | password 
----------+----------
(0 rows)
     
we get no match and nothing is returned.

# Video 11: Learning the rudiments of sessions and passwords

This video described the super search rudiment. Here are some things to consider:

## converting a mySQL SQL file to a PostgreSQL one
Suppose my MySQL file looks like this:

     CREATE DATABASE IF NOT EXISTS pets;
     USE pets;
     
     CREATE TABLE IF NOT EXISTS frogs (
         `id` int(11) NOT NULL AUTO_INCREMENT,
         `name` varchar(70) NOT NULL,
         `species` varchar(50) NOT NULL,
         `age` int(3) NOT NULL,
         PRIMARY KEY (`id`)
     ) ENGINE=MyISAM  DEFAULT CHARSET=latin1 AUTO_INCREMENT=4 ;
     
     INSERT INTO frogs (id, name, species, age) VALUES 
           (1, 'Froggy', 'Blue Dart', 2),
           (2, 'Sammy', 'Yellow Banded Poison Arrow', 4),
           (3, 'Dido', 'Standard Tree', 3);
           
How do we convert this to a Postgresql compatible file? Here are a few things to note:

1. the `IF NOT EXISTS` on the create database line does not exist in Postgresql. We need to remove it.
2. Postgresql does not use `USE` and instead uses `\c`
2. Postgres handles auto incrementing fields differently and uses the `serial` datatype rather than the `AUTO_INCREMENT` tag. 
3. The term *precision* refers to the number of digits in a number and *scale* refers to the number of digits to the right of the decimal point. Postgresql does not allow us to specify the precision of an integer (as in the above line for age with `int(3)`). There are 2 alternatives, we can either change the datatype to `int` or use `numeric(3)`. With the numeric datatype you can specify both precision and scale with `numeric(precision, scale) ` For example `numeric(5,2)`
4. Everything after the closing ) should be removed.
5. Since the `id` field will now be a serial, when we do our insert we need to not include this field.

With that in mind, here is the Postgres version of the above.

       DROP DATABASE pets;
       CREATE DATABASE  pets;
       \c pets;
     
     CREATE TABLE IF NOT EXISTS frogs (
         id serial,
         name varchar(70) NOT NULL,
         species varchar(50) NOT NULL,
         age`int(3) NOT NULL,
         PRIMARY KEY (`id`)
     );
     
     INSERT INTO frogs (name, species, age) VALUES 
           ('Froggy', 'Blue Dart', 2),
           ('Sammy', 'Yellow Banded Poison Arrow', 4),
           ('Dido', 'Standard Tree', 3);
    

# Video 12: SocketIO Part 1: AngularJS and flask-socketio

## Model View Controller - hands on

## 
AngularJS
Runs in the browser.

So to use SocketIO you need something running in the browser (which will be AngularJS) and something server side (which will be Python/Flask Socketio)

     
## Installing Flask-Socketio

     sudo easy_install flask-socketio
    
Getting the github repository for this demo:

     git clone https://github.com/zacharski/ISSchat.git
 

Here are the steps to take to implement a simple chat app:

### 1. import libraries
    
Now in my index.html file I will add the following 2 lines to import some javascript libraries (the angular and socketio ones):

    <script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/angularjs/1.4.8/angular.min.js"></script>
    <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/1.4.4/socket.io.js"></script>

### 2. next indicate the the web page is an angular app by modifying the html tag from `<html>` to
     <html ng-app="ISSChatApp">
     
that `ISSChatApp` can be any name you want. I will show you what it connects to shortly.

### 3. We need to modify the username field, the scrolling window that shows the messages, and a few other tags:

        <input type="text"   ng-model="name" ng-change="setName()" placeholder="Username" size="14" /></p>

In the video I illustrated this by changing the div below this to:

     <div class="scroll" id="msgpane">
     <p>{{name}}</p>
     </div>
Then when [I typed something into the input field we see it echoed in the div area.](https://youtu.be/5cQFzc_Zo8M?list=PLuZfoSIficQvVN44F1emhKAF-1KIoRcok&t=484) We also see that Angular variables look like variables in Flask templates.

### 4. In the demo I create a javascript file (controller.js)

     var ISSChatApp = angular.module('ISSChatApp', []);

    ISSChatApp.controller('ChatController', function($scope){
    var socket = io.connect('https://' + document.domain + ':' 
    +location.port + '/iss'); 
    
    $scope.messages = [];
    $scope.name = '';
    $scope.text = '';
    

    socket.on('message', function(msg){
       console.log(msg);
       $scope.messages.push(msg);
       $scope.$apply();
       var elem = document.getElementById('msgpane');
       elem.scrollTop = elem.scrollHeight;
        
    });

* In this example, the namespace is `iss`. This is important to know.
* The variables I am using are prefixed with `$scope.` 
* The `console.log(msg)` prints to the javascript console **in the browser**

Next I will import this javascript file in my html file:
 
     <script src="js/controller.js"></script>
     
Then I associate the angular app I created in the javascript file to the html code:

     <html ng-app="ISSChatApp">
     

Next, I associate the controller (see the name I gave it in the javascript code above) to the html code:

     <div class="container" ng-controller="ChatController">
     
## Now the Python side

### uuid  universal unique identifiers.

We are going to make use of Python's uuid module. As the name implies, it generates unique identifiers. Here is an example

     >>> import uuid
     >>> uuid.uuid1()
     UUID('a018b2ba-7ec8-11e6-8d1d-7831c1cca2c6')
     >>> uuid.uuid1()
     UUID('a13799d8-7ec8-11e6-b71f-7831c1cca2c6')
     >>> uuid.uuid1()
     UUID('a539e3e2-7ec8-11e6-8fbd-7831c1cca2c6')
     >>> uuid.uuid1()
     UUID('a70c1f62-7ec8-11e6-adf2-7831c1cca2c6')
     
we are going to use this to keep track of unique users of our site

So in my server.py file I will import that library and the socketio library:

     import uuid
     from flask.ext.socketio import SocketIO, emit
     
then I create the socketio app:

     socketio = SocketIO(app)
     
Now I will make a few test messages stored in a list:

     messages = [{'text': 'Booting system', 'name': 'Bot'},
            {'text': 'ISS Chat now live!', 'name': 'Bot'}]

     users = {}
     
The run command at the bottom of the file I change to:

      # start the server
     if __name__ == '__main__':
        socketio.run(app, host=os.getenv('IP', '0.0.0.0'), port =int(os.getenv('PORT', 8080)), debug=True)

### now I will show handling one event--the connect event

      @socketio.on('connect', namespace='/iss')
      def makeConnection():
         session['uuid'] = uuid.uuid1()
         session['username'] = 'New user'
         print('connected')
         users[session['uuid']] = {'username': 'New user'}
    
         for message in messages:
             print(message)
             emit('message', message)


The emit function takes the following arguments:

       emit(eventName, arguments)
       
so 

       emit('message', message)
       
sends an event named `message` to the javascript client side with the value of the variable message. It only sends it to the one user. If we use the optional `broadcast = True` the event will be sent to all connected users.

     emit('message', tmp, broadcast=True)
     
### IMPORTANT:

Since I emit an event `message` on the Python side, I need a corresponding 

     socket.on('message', function(msg){
     
in the Javascript file. (If you scroll up a bit to the Javascript code, you will see that we do.)  The reverse is also true. If we have on the Javascript side:

     socket.emit('identify', $scope.name)

So Javascript is emitting an event named 'identify'. We need a corresponding 

     @socketio.on('identify', namespace='/iss')
     
on the Python side. 

Also the namespace in that Python line needs to match the namespace defined in the Javascript file.

