
## How to install Apache Spark on CentOS Standalone

#### Install Java 
``` bash
yum install java -y
# Verify Java installation
java -version
```

#### Install Scala
``` bash
yum install wget -y
wget http://downloads.typesafe.com/scala/2.11.7/scala-2.11.7.tgz
tar xvf scala-2.11.7.tgz
sudo mv scala-2.11.7 /usr/lib
sudo ln -s /usr/lib/scala-2.11.7 /usr/lib/scala
# Set PATH for Scala
export PATH=$PATH:/usr/lib/scala/bin
# Verify Scala installation
scala -version
```

#### Install Apache Spark
``` bash
wget http://d3kbcqa49mib13.cloudfront.net/spark-2.0.0-bin-hadoop2.7.tgz
tar xvf spark-2.0.0-bin-hadoop2.7.tgz
sudo mv spark-2.0.0-bin-hadoop2.7 /usr/local/spark
# Set up the environment for Spark
export SPARK_HOME=/usr/local/spark
export PATH=$PATH:$SPARK_HOME/bin
source ~/.bash_profile
# Verify the Spark Installation
spark-shell
```

Turn default logging down.
``` bash
cd /usr/local/spark/conf
cp log4j.properties.template log4j.properties
vi log4j.properties
```
Change `log4j.rootCategory=INFO, console` to `log4j.rootCategory=WARN, console`.


#### Configure firewall
``` bash
firewall-cmd --permanent --zone=public --add-port=6066/tcp
firewall-cmd --permanent --zone=public --add-port=7077/tcp
firewall-cmd --permanent --zone=public --add-port=8080-8081/tcp
firewall-cmd --reload
```

#### Resources
- [Spark Configuration](http://spark.apache.org/docs/latest/configuration.html)
- [Spark Examples](http://spark.apache.org/examples.html)