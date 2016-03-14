
#### 1. the Cassandra client is cqlsh

     $ cqlsh
     Connected to Test Cluster
     cqlsh> 
####2. create a keyspace (like a database)
     CREATE KEYSPACE umw
     WITH REPLICATION = { 'class' : 'SimpleStrategy', 
                          'replication_factor' : 1 };
                          
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