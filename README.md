Main tutorial source for current / updated information - [Nutch2Tutorial](https://wiki.apache.org/nutch/Nutch2Tutorial)

### Nutch - apache-nutch-2.3.1-src[Download Link](http://www.apache.org/dyn/closer.cgi/nutch/)
### HBase - hbase-0.98.8-hadoop2-bin-[Download Link](http://archive.apache.org/dist/hbase/hbase-0.98.8/hbase-0.98.8-hadoop2-bin.tar.gz) 
### Ant - ANT 1.10.1
### Solr - Apache Solr solr-5.2.1
### ZooKeeper - zookeeper-3.4.10
### Apache Gora - Need not be downloaded separately - [Apache Project Page](http://gora.apache.org/)

Download all Binary Archives usually in Gzip formats - Untar all of them in a NUTCH Directory ( Mine is Called Nutch1)
Update your Java Home path in your .bashrc file . This can be edited with a text editor - i use Gedit on Ubuntu.

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

Next Open the file --- ~/Nutch1/hbase/conf/hbase-site.xml
#
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
We need to ensure GORA uses HBase as its Default Data Store. 
#
Open the file -- ~/Nutch1/nutch/conf/gora.properties 
Find the Commented 
```
#########################
# HBaseStore properties #
#########################
```
Just below this commented section - Add Line -- ``` gora.datastore.default=org.apache.gora.hbase.store.HBaseStore ```
#
Next we configure IVY - Ivy is the now de-facto [dependency management tool used for Nutch Builds](https://issues.apache.org/jira/browse/NUTCH-821)
#
Open File -- $NUTCH_HOME/ivy/ivy.xml  -- in my case its - /home/dhankar/Nutch1/nutch/ivy/ivy.xml
Uncomment and add lines as below -- 

```
<!-- Uncomment this to use HBase as Gora backend. -->

    <dependency org="org.apache.gora" name="gora-hbase" rev="0.6.1" conf="*->default" /> 

    <dependency org="org.apache.hbase" name="hbase-common" rev="0.98.8-hadoop2" conf="*->default" />
```
#




