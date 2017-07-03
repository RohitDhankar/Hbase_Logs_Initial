### Nutch - apache-nutch-2.3.1-src
### HBase - hbase-0.98.8-hadoop2-bin
### Ant - ANT 1.10.1
### Solr - Apache Solr solr-5.2.1
### ZooKeeper - zookeeper-3.4.10

```
$ sudo gedit ~/.bashrc
```

in the bashrc file --- add PATH Variables at end of file --- 

```
export JAVA_HOME=" Abs Path to JDK "
export NUTCH_JAVA_HOME=" Abs Path "
```


After Updating the PATH Variables we need to SOURCE the .bashrc File --- this is done without SUDO 

```
$ source ~/.bashrc

```
Saved by SOURCING the .bashrc file 
#

Open file --- /home/dhankar/Nutch1/hbase/conf/hbase-site.xml
Add ```<property>``` Tags within ```<configuration>``` Tags - these are for the HBase Root Directory 
and the --- hbase.zookeeper.property.dataDir
#
Note carefully the Syntax for - file:///home

```
<configuration>

<property>
        <name>hbase.rootdir</name>
        <value>file:///home/dhankar/Nutch1/hbase</value>
    </property>
    <property>
        <name>hbase.zookeeper.property.dataDir</name>
        <value>/home/dhankar/Nutch1/zookeeper</value>
    </property>


</configuration>
```

##
Next we need to Configure [APACHE - Gora](http://gora.apache.org/)

#






