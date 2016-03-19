# Cassandra: quick start

### Installing on Cloud9

1. Create a new Cloud9 workspace
2.  Follow the instructions [here](http://docs.datastax.com/en/cassandra/2.0/cassandra/install/installDeb_t.html) skipping step 3.
2. In Step 6 of those instructions, once you stop the server, and in addition to removing .
  1. as started in those instructions, delete `/var/lib/cassandra/data/system/*`
  2. using `sudo vim /etc/init.d/cassandra` comment out the `ulimit -l unlimited` line and save the file.
  3. using sudo vim /etc/cassandra/cassandra.yaml change the line `authenticator: AllowAllAuthenticator` to `authenticator: PasswordAuthenticator` The default before is to allow people to login to the server without a password, this change will require a username and password to login (default username and password are 'cassandra') See the details [here](http://docs.datastax.com/en/cassandra/2.1/cassandra/security/security_config_native_authenticate_t.html)
  4. in the same file change `authorizer: AllowAllAuthorizer` to
`authorizer: org.apache.cassandra.auth.CassandraAuthorizer` then save the file.
4. start the cassandra server `sudo service cassandra start`  and make sure it is running `sudo service cassandra status`

#### Congratulations!!

### quick tutorial

#### 1. the Cassandra client is cqlsh

     $ cqlsh -u cassandra -p cassandra
     Connected to Test Cluster
     cqlsh> 
####2. create a keyspace (like a database)
     CREATE KEYSPACE umw
     WITH REPLICATION = { 'class' : 'SimpleStrategy', 'replication_factor' : 1 };
                          
#### 3.  use the keyspace
     USE umw;

#### 4. create a table

     CREATE TABLE pets (name text, 
                        breed text, 
                        age int, 
                        PRIMARY KEY (name));

#### 5.  describe a table

     DESCRIBE TABLE pets;

#### 6. inserting entries

     INSERT INTO pets (name, breed, age) VALUES 
                 ('Bodhi', 'Standard Poodle', 4);
     INSERT INTO pets (name, breed, age) VALUES 
                 ('Watson', 'Border Collie', 12);
     INSERT INTO pets (name, breed, age) VALUES 
                 ('Roz', 'Standard Poodle', 9);
     INSERT INTO pets (name, breed, age) VALUES 
                 ('Frieda', 'German Shepherd', 7);
     
#### 7. you can find things based on their primary key

     SELECT * FROM pets WHERE name = 'Bodhi';
      name  | age | breed                                                                
     -------+-----+-----------------                                                     
      Bodhi |   4 | Standard Poodle     


     SELECT * FROM pets WHERE breed = 'Standard Poodle';
     InvalidRequest: code=2200 [Invalid query] message="No secondary indexes on the restricted columns support the provided operators: "                                                    

#### 8. update

    UPDATE pets SET age = 10 WHERE name = 'Roz';

#### 9. creating indices

     CREATE INDEX on pets (breed; 



## getting information from postgres.

#### example of creating a new table:

     select c.id, c.name, c.district, c.countrycode, d.name country, c.population into newcity from city c join country d on c.countrycode = d.code;

#### example of saving a table to a text file.

     pg_dump -U postgres -h localhost -t newcity --column-inserts world > newcity2.sql


edit this

## Python and Cassandra

#### install the Cassandra Python driver:


    pip install cassandra-driver

#### simple example of its use:

     from cassandra.cluster import Cluster
     from cassandra.auth import PlainTextAuthProvider
    auth_provider = PlainTextAuthProvider(
    username = 'carmen', password='kirbyk9')
    cluster = Cluster(auth_provider=auth_provider)
    
    session = cluster.connect('world')
    rows = session.execute("select city, district,    country from city where district = 'Arizona'")
    for row in rows:
        print row.city, row.district, row.country
 
> Written with [StackEdit](https://stackedit.io/).
> 