Cassandra: quick start

installing Cassandra on Cloud9 - this is what I tried!

sudo echo "deb http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main" | sudo tee /etc/apt/sources.list.d/webupd8team-java.list

sudo echo "deb-src http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main" |sudo  tee -a /etc/apt/sources.list.d/webupd8team-java.list

sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys EEA14886

sudo apt-get update
sudo apt-get install oracle-java8-installer


echo "deb http://debian.datastax.com/community stable main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list


curl -L http://debian.datastax.com/debian/repo_key | sudo apt-key add -

sudo apt-get update

sudo apt-get install cassandra


sudo service cassandra status
and it still doesn’t work.

installing Cassandra in vagrant or a web-based vm:

digitalOcean’s Guide

quick tutorial

1. the Cassandra client is cqlsh

 $ cqlsh
 Connected to Test Cluster
 cqlsh> 
2. create a keyspace (like a database)

 CREATE KEYSPACE umw
 WITH REPLICATION = { 'class' : 'SimpleStrategy', 
                      'replication_factor' : 1 };
3. use the keyspace

 USE umw;
4. create a table

 CREATE TABLE pets (name text, 
                    breed text, 
                    age int, 
                    PRIMARY KEY (name));
5. describe a table

 DESCRIBE TABLE pets;
6. inserting entries

 INSERT INTO pets (name, breed, age) VALUES 
             ('Bodhi', 'Standard Poodle', 4);
 INSERT INTO pets (name, breed, age) VALUES 
             ('Watson', 'Border Collie', 12);
 INSERT INTO pets (name, breed, age) VALUES 
             ('Roz', 'Standard Poodle', 9);
 INSERT INTO pets (name, breed, age) VALUES 
             ('Frieda', 'German Shepherd', 7);
7. you can find things based on their primary key

 SELECT * FROM pets WHERE name = 'Bodhi';
  name  | age | breed                                                                
 -------+-----+-----------------                                                     
  Bodhi |   4 | Standard Poodle     


 SELECT * FROM pets WHERE breed = 'Standard Poodle';
 InvalidRequest: code=2200 [Invalid query] message="No secondary indexes on the restricted columns support the provided operators: "                                                    
8. update

UPDATE pets SET age = 10 WHERE name = 'Roz';
9. creating indices

 CREATE INDEX on pets (breed; 