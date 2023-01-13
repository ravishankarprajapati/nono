# P1 Installing and setting environment variables for Working with Apache Hadoop. 

Theory: 

Apache Hadoop is an open-source software framework used to store, manage and process large datasets for various big data computing applications running under clustered systems. It is Java-based and uses Hadoop Distributed File System (HDFS) to store its data and process data using MapReduce. 
Implementation: 

# Installing Java 

1 . In Ubuntu, open the terminal and enter the following command: 
	
	$ sudo apt install default-jdk default-jre -y 

2. Verify the installation via 
	
	$ java -version 
* Create new user Hadoop and configure password-less SSH 

1 . Create the new user 
	$ sudo adduser hadoop 
2 . Add the hadoop user to the sudo group. 
	$ sudo usermod -aG sudo hadoop 
3 . Log out of the current user and switch to the newly created Hadoop user 
4 . In terminal, install openssh client and server 
	$ apt install openssh-server openssh-client -y 
5 . Generate public and private key pairs. 
	$ ssh-keygen -t rsa 
6 . Add the generated public key from id_rsa.pub to authorized_keys. 
	$ sudo cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys 
7 . Change the permissions of the authorized_keys file. 
	$ sudo chmod 640 ~/.ssh/authorized_keys 
8 . Verify if the password-less SSH is functional. 
	$ ssh localhost


# Install Apache Hadoop 
1 . Download the latest stable version of Hadoop. To get the latest version, go to Apache Hadoop official download page. 
2 . Extract the downloaded file. 
	$ tar -xvzf hadoop-3.3.1.tar.gz 
3. Move the extracted directory to the /usr/local/ directory. 
	$ sudo mv hadoop-3.3.1 /usr/local/hadoop 
4. Create directory to store system logs. 
	$ sudo mkdir /usr/local/hadoop/logs 
5. Change the ownership of the hadoop directory. 
	$ sudo chown -R hadoop:hadoop /usr/local/Hadoop
# Configure Hadoop

1. Edit file ~/.bashrc to configure the Hadoop environment variables.

	$ sudo nano ~/.bashrc

2. Add the following lines to the file. Save and close the file.
	
	export HADOOP_HOME=/usr/local/hadoop
	
	export HADOOP_INSTALL=$HADOOP_HOME
	
	export HADOOP_MAPRED_HOME=$HADOOP_HOME
	
	export HADOOP_COMMON_HOME=$HADOOP_HOME
	
	export HADOOP_HDFS_HOME=$HADOOP_HOME
	
	export YARN_HOME=$HADOOP_HOME
	
	export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
	
	export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
	
	export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib/native"

3. Add the changes to the memory

	$ source ~/.bashrc
