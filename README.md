## Welcome to CS350

# Video 1: Getting Started with Cloud9
No summary provided.

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

     @app.route('/registration2', method=['POST'])
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


### installing the psycopg2 module:

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

