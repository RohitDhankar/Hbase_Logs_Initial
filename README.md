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
Next we Compile Nutch Via the ANT Runtime ---
```
ant runtime 
```
Running above cmd - we may get a delayed / staggered response - the content of the console should be as below -- 

```
dhankar@dhankar-VPCEB44EN:~/Nutch1/nutch$ ant -version
Apache Ant(TM) version 1.10.1 compiled on February 2 2017
dhankar@dhankar-VPCEB44EN:~/Nutch1/nutch$ 
dhankar@dhankar-VPCEB44EN:~/Nutch1/nutch$ ant runtime
Buildfile: /home/dhankar/Nutch1/nutch/build.xml
Trying to override old definition of task javac
  [taskdef] Could not load definitions from resource org/sonar/ant/antlib.xml. It could not be found.

ivy-probe-antlib:

ivy-download:
  [taskdef] Could not load definitions from resource org/sonar/ant/antlib.xml. It could not be found.

ivy-download-unchecked:

ivy-init-antlib:

ivy-init:

init:

clean-lib:

resolve-default:
[ivy:resolve] :: Apache Ivy 2.3.0 - 20130110142753 :: http://ant.apache.org/ivy/ ::
[ivy:resolve] :: loading settings :: file = /home/dhankar/Nutch1/nutch/ivy/ivysettings.xml
[ivy:resolve] downloading http://repo1.maven.org/maven2/org/apache/solr/solr-solrj/4.6.0/solr-solrj-4.6.0.jar ...
[ivy:resolve] ...................................................................................................................................... (393kB)
[ivy:resolve] .. (0kB)
[ivy:resolve] 	[SUCCESSFUL ] org.apache.solr#solr-solrj;4.6.0!solr-solrj.jar (847ms)
[ivy:resolve] downloading http://repo1.maven.org/maven2/org/apache/hadoop/hadoop-core/1.2.0/hadoop-core-1.2.0.jar ...
[ivy:resolve] ..................................................................................................................................................................................................................................................................................................
[ivy:resolve] .... (4101kB)
[ivy:resolve] .. (0kB)
[ivy:resolve] 	[SUCCESSFUL ] org.apache.hadoop#hadoop-core;1.2.0!hadoop-core.jar (2371ms)
[ivy:resolve] downloading http://repo1.maven.org/maven2/org/jdom/jdom/1.1/jdom-1.1.jar ...
[ivy:resolve] ......................... (149kB)
[ivy:resolve] .. (0kB)
[ivy:resolve] 	[SUCCESSFUL ] org.jdom#jdom;1.1!jdom.jar (623ms)
[ivy:resolve] downloading http://maven.restlet.org/org/restlet/jse/org.restlet/2.2.3/org.restlet-2.2.3.jar ...
[ivy:resolve] ................
[ivy:resolve] ..........................
[ivy:resolve] ............................
[ivy:resolve] ..........................
[ivy:resolve] ...........................................
[ivy:resolve] .......................... (670kB)
[ivy:resolve] .. (0kB)
[ivy:resolve] 	[SUCCESSFUL ] org.restlet.jse#org.restlet;2.2.3!org.restlet.jar (13164ms)
[ivy:resolve] downloading http://maven.restlet.org/org/restlet/jse/org.restlet.ext.jackson/2.2.3/org.restlet.ext.jackson-2.2.3.jar ...
[ivy:resolve] .. (7kB)
[ivy:resolve] .. (0kB)
[ivy:resolve] 	[SUCCESSFUL ] org.restlet.jse#org.restlet.ext.jackson;2.2.3!org.restlet.ext.jackson.jar (3404ms)
[ivy:resolve] downloading http://maven.restlet.org/org/restlet/jse/org.restlet.ext.jaxrs/2.2.3/org.restlet.ext.jaxrs-2.2.3.jar ...
[ivy:resolve] ..........................................
[ivy:resolve] . (305kB)
[ivy:resolve] .. (0kB)

### Continued ---------- Truncated Dump of Console 
```
#
Next we want to start up HBase - 
CD into the Nutch Root DIR - Nutch1 in my case - run cmd ---> 
``` hbase/bin/start-hbase.sh```
#
My console output as below -- 
#
```
dhankar@dhankar-VPCEB44EN:~/Nutch1$ hbase/bin/start-hbase.sh
starting master, logging to /home/dhankar/Nutch1/hbase/bin/../logs/hbase-dhankar-master-dhankar-VPCEB44EN.out

```
#







