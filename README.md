# P1 Installing and setting environment variables for Working with Apache Hadoop. 

Theory: 

Apache Hadoop is an open-source software framework used to store, manage and process large datasets for various big data computing applications running under clustered systems. It is Java-based and uses Hadoop Distributed File System (HDFS) to store its data and process data using MapReduce. 
Implementation: 


* Installing Java 

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

<configuration>
	
	<property>
	
		<name>mapreduce.framework.name</name>
	
		<value>yarn</value>
	</property>

	<property>

		<name>yarn.app.mapreduce.am.env</name>

		<value>HADOOP_MAPRED_HOME=/usr/local/hadoop</value>

		<description>Change this to your hadoop location.</description>
	</property>

	<property>

		<name>mapreduce.map.env</name>

		<value>HADOOP_MAPRED_HOME=/usr/local/hadoop</value>

		<description>Change this to your hadoop location.</description>
	
	</property>

	<property>

		<name>mapreduce.reduce.env</name>

		<value>HADOOP_MAPRED_HOME=/usr/local/hadoop/<value>

		<description>Change this to your hadoop location.</description>

	</property>
</configuration>

14. Edit the yarn-site.xml configuration file and define YARN-related settings.

	$ sudo nano $HADOOP_HOME/etc/hadoop/yarn-site.xml

15. Add the following lines. Save and close the file.
	
	<configuration>
	
		<property>
		
			<name>yarn.nodemanager.aux-services</name>

			<value>mapreduce_shuffle</value>
	
		</property>
	
	</configuration>

16. Validate the Hadoop configuration and format the HDFS NameNode.

	$ hdfs namenode -format
* Start the Apache Hadoop Cluster

1. Start the NameNode and DataNode.
	
	$ start-dfs.sh

2. Start the YARN resource and node managers.

	$ start-yarn.sh

3. Verify all the running components.

	$ jps

	
5 . Generate public and private key pairs. 
	
	$ ssh-keygen -t rsa 
	
6 . Add the generated public key from id_rsa.pub to authorized_keys. 

	$ sudo cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys 
	
7 . Change the permissions of the authorized_keys file. 

	$ sudo chmod 640 ~/.ssh/authorized_keys 
	
8 . Verify if the password-less SSH is functional. 

	$ ssh localhost


* Install Apache Hadoop 

1 . Download the latest stable version of Hadoop. To get the latest version, go to Apache Hadoop official download page. 

2 . Extract the downloaded file. 

	$ tar -xvzf hadoop-3.3.1.tar.gz 

3. Move the extracted directory to the /usr/local/ directory. 
	
	$ sudo mv hadoop-3.3.1 /usr/local/hadoop 

4. Create directory to store system logs. 
	
	$ sudo mkdir /usr/local/hadoop/logs 

5. Change the ownership of the hadoop directory. 
	
	$ sudo chown -R hadoop:hadoop /usr/local/Hadoop
* Configure Hadoop

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

* Configure Java Environment Variables

1. Find the OpenJDK directory and copy the output

	$ readlink -f /usr/bin/javac

2. Edit the hadoop-env.sh file.
	
	$ sudo nano $HADOOP_HOME/etc/hadoop/hadoop-env.sh

3. Add the following lines to the file. Then, close and save the file.
	
	export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64

	export HADOOP_CLASSPATH+=" $HADOOP_HOME/lib/*.jar"

4. Browse to the hadoop lib directory.
	$ cd /usr/local/hadoop/lib
	
5. Download the Javax activation file.

	$ sudo wget https://jcenter.bintray.com/javax/activation/javax.activationapi/1.2.0/javax.activation-api-1.2.0.jar

6. Verify the Hadoop version.

	$ hadoop version

7. Edit the core-site.xml configuration file to specify the URL for your NameNode.

	$ sudo nano $HADOOP_HOME/etc/hadoop/core-site.xml

8. Add the following lines. Save and close the file.

	<configuration>
		
		<property>
			
			<name>fs.default.name</name>
			
			<value>hdfs://0.0.0.0:9000</value>
			
			<description>The default file system URI</description>
		
		</property>
	
	</configuration>

9. Create a directory for storing node metadata and change the ownership to hadoop.

	$ sudo mkdir -p /home/hadoop/hdfs/{namenode,datanode}

	$ sudo chown -R hadoop:hadoop /home/hadoop/hdfs

10. Edit hdfs-site.xml configuration file to define the location for storing node metadata, fsimage file.

	$ sudo nano $HADOOP_HOME/etc/hadoop/hdfs-site.xml

11. Add the following lines. Close and save the file.
	
	<configuration>
	
		<property>
			
			<name>dfs.replication</name>
			
			<value>1</value>
			
		</property>
	
		<property>
			
			<name>dfs.name.dir</name>
			
			<value>file:///home/hadoop/hdfs/namenode</value>
			
		</property>
	
		<property>
			
			<name>dfs.data.dir</name>
			
			<value>file:///home/hadoop/hdfs/datanode</value>
			
		</property>
	
		<property>
			
			<name>dfs.permissions.enabled</name>
			
			<value>false</value>
			
			</property>
	
	</configuration>

12. Edit mapred-site.xml configuration file to define MapReduce values.

	$ sudo nano $HADOOP_HOME/etc/hadoop/mapred-site.xml

13. Add the following lines. Save and close the file

<configuration>
	
	<property>
	
		<name>mapreduce.framework.name</name>
	
		<value>yarn</value>
	</property>

	<property>

		<name>yarn.app.mapreduce.am.env</name>

		<value>HADOOP_MAPRED_HOME=/usr/local/hadoop</value>

		<description>Change this to your hadoop location.</description>
	</property>

	<property>

		<name>mapreduce.map.env</name>

		<value>HADOOP_MAPRED_HOME=/usr/local/hadoop</value>

		<description>Change this to your hadoop location.</description>
	
	</property>

	<property>

		<name>mapreduce.reduce.env</name>

		<value>HADOOP_MAPRED_HOME=/usr/local/hadoop/<value>

		<description>Change this to your hadoop location.</description>

	</property>
</configuration>

14. Edit the yarn-site.xml configuration file and define YARN-related settings.

	$ sudo nano $HADOOP_HOME/etc/hadoop/yarn-site.xml

15. Add the following lines. Save and close the file.
	
	<configuration>
	
		<property>
		
			<name>yarn.nodemanager.aux-services</name>

			<value>mapreduce_shuffle</value>
	
		</property>
	
	</configuration>

16. Validate the Hadoop configuration and format the HDFS NameNode.

	$ hdfs namenode -format
* Start the Apache Hadoop Cluster

1. Start the NameNode and DataNode.
	
	$ start-dfs.sh

2. Start the YARN resource and node managers.

	$ start-yarn.sh

3. Verify all the running components.

	$ jps

* Access Apache Hadoop Web Interface

In your web browser enter the URLs given below to access the interface

		http://localhost:9870

		http://localhost:8088
