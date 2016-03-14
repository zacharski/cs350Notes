## Getting started with Cassandra

### installing Cassandra on Cloud9

    sudo echo "deb http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main" | sudo tee /etc/apt/sources.list.d/webupd8team-java.list

    sudo echo "deb-src http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main" |sudo  tee -a /etc/apt/sources.list.d/webupd8team-java.list

    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys EEA14886

    sudo apt-get install oracle-java8-installer


    echo "deb http://debian.datastax.com/community stable main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list


    curl -L http://debian.datastax.com/debian/repo_key | sudo apt-key add -

    sudo apt-get update

    sudo apt-get install cassandra


    sudo service cassandra status