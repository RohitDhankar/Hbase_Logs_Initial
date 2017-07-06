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

in the bashrc file - add PATH Variable for NUTCH_JAVA_HOME at end of file - this has to be the same Path Variable that we have provided for JAVA_HOME within this very file earlier --->

```
## Need to set the -- NUTCH_JAVA_HOME= same as --- JAVA_HOME 
#
export NUTCH_JAVA_HOME=/home/dhankar/usr/lib/jvm/java-8-oracle/

```
After Updating the PATH Variables we need to SOURCE the .bashrc File --- this is done without SUDO 

```
$ source ~/.bashrc
```
Saved by SOURCING the .bashrc file 
#
Next we open file ```~/Nutch1/hbase/conf/hbase-env.sh```  need to mention JAVA_HOME same as .bashrc file here too .

```
#### JAVA_HOME path as below -- in some sites mentioned as "JAVA_HOME=usr/ " only - but i have entire path
export JAVA_HOME=/home/dhankar/usr/lib/jvm/java-8-oracle/
```

We also uncomment line as below - so that HBase can manage its own instances of Zookeeper , we need not manually START or STOP Zookeeper. 

```
# Tell HBase whether it should manage it's own instance of Zookeeper or not.
export HBASE_MANAGES_ZK=true

```
#
Next if we are using HBase version less than - 0.96 We open the --- ``` sudo gedit /etc/hosts ```

Change from as below to further as below --- "YourUbuntuComputerName" == 127.0.1.1 ---> 127.0.0.1
```

127.0.0.1	localhost
127.0.1.1	YourUbuntuComputerName


```
#
```

127.0.0.1	localhost
127.0.0.1	YourUbuntuComputerName


```
#
In my case i am using HBase 0.98.8.hadoop-2 ---- thus as mentioned on HBase site and i quote verbatim below -  i need not make any changes to my ``` sudo gedit /etc/hosts   ```

QUOTE -- "This issue has been fixed in hbase-0.96.0 and beyond." --- UNQUOTE

#

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
#
Have seen certain folk having configured the Deafult Port for Zookeeper Local Host as - 2222
Am not sure if this is required - this step has been by-passed for now. 

```
<property>
      <name>hbase.zookeeper.property.clientPort</name>
          <value>
              2222
          </value>
             <description>
                Local Host Value for Zookeeper connection .
             </description>
</property>

```
#
I cant see a reason why we should change the default Port - 2181 , thus have made an entry in file -- ```  hbase-site.xml```
which just confirms the Port as - 2181 
```
<property>
      <name>hbase.zookeeper.property.clientPort</name>
      <value>2181</value>
      <description>Property from ZooKeeper's config zoo.cfg.
      The port at which the clients will connect.
      </description>
</property>
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
resolve-default:
[ivy:resolve] :: loading settings :: file = /home/dhankar/Nutch1/nutch/ivy/ivysettings.xml

compile:

jar:

deps-test:

deploy:

copy-generated-lib:

deploy:
     [copy] Copying 1 file to /home/dhankar/Nutch1/nutch/build/plugins/urlnormalizer-regex

copy-generated-lib:
     [copy] Copying 1 file to /home/dhankar/Nutch1/nutch/build/plugins/urlnormalizer-regex

compile:

job:
      [jar] Building jar: /home/dhankar/Nutch1/nutch/build/apache-nutch-2.3.1.job

runtime:
    [mkdir] Created dir: /home/dhankar/Nutch1/nutch/runtime
    [mkdir] Created dir: /home/dhankar/Nutch1/nutch/runtime/local
    [mkdir] Created dir: /home/dhankar/Nutch1/nutch/runtime/deploy
     [copy] Copying 1 file to /home/dhankar/Nutch1/nutch/runtime/deploy
     [copy] Copying 2 files to /home/dhankar/Nutch1/nutch/runtime/deploy/bin
     [copy] Copying 1 file to /home/dhankar/Nutch1/nutch/runtime/local/lib
     [copy] Copying 1 file to /home/dhankar/Nutch1/nutch/runtime/local/lib/native
     [copy] Copying 30 files to /home/dhankar/Nutch1/nutch/runtime/local/conf
     [copy] Copying 2 files to /home/dhankar/Nutch1/nutch/runtime/local/bin
     [copy] Copying 228 files to /home/dhankar/Nutch1/nutch/runtime/local/lib
     [copy] Copying 189 files to /home/dhankar/Nutch1/nutch/runtime/local/plugins
     [copy] Copied 2 empty directories to 2 empty directories under /home/dhankar/Nutch1/nutch/runtime/local/test

BUILD SUCCESSFUL
Total time: 7 minutes 12 seconds
dhankar@dhankar-VPCEB44EN:~/Nutch1/nutch$ 

```
# 
The console dump as seen above may differ for you - focus on the - BUILD SUCCESSFUL. 
Also we can run certain basic tests to see Nutch has been Built Successfully 
cd into DIR -- ``` nutch/runtime/local ```
#
```
dhankar@dhankar-VPCEB44EN:~/Nutch1/nutch/runtime/local$ bin/nutch
Usage: nutch COMMAND
where COMMAND is one of:
 inject		inject new urls into the database
 hostinject     creates or updates an existing host table from a text file
 generate 	generate new batches to fetch from crawl db
 fetch 		fetch URLs marked during generate
 parse 		parse URLs marked during fetch
 updatedb 	update web table after parsing
 updatehostdb   update host table after parsing
 readdb 	read/dump records from page database
 readhostdb     display entries from the hostDB
 index          run the plugin-based indexer on parsed batches
 elasticindex   run the elasticsearch indexer - DEPRECATED use the index command instead
 solrindex 	run the solr indexer on parsed batches - DEPRECATED use the index command instead
 solrdedup 	remove duplicates from solr
 solrclean      remove HTTP 301 and 404 documents from solr - DEPRECATED use the clean command instead
 clean          remove HTTP 301 and 404 documents and duplicates from indexing backends configured via plugins
 parsechecker   check the parser for a given url
 indexchecker   check the indexing filters for a given url
 plugin 	load a plugin and run one of its classes main()
 nutchserver    run a (local) Nutch server on a user defined port
 webapp         run a local Nutch web application
 junit         	runs the given JUnit test
 or
 CLASSNAME 	run the class named CLASSNAME
Most commands print help when invoked w/o parameters.
dhankar@dhankar-VPCEB44EN:~/Nutch1/nutch/runtime/local$ 
dhankar@dhankar-VPCEB44EN:~/Nutch1/nutch/runtime/local$ bin/nutch webapp
********************************************************************
*** WARNING: Wicket is running in DEVELOPMENT mode.              ***
***                               ^^^^^^^^^^^                    ***
*** Do NOT deploy to your live server(s) without changing this.  ***
*** See Application#getConfigurationType() for more information. ***
********************************************************************

```
#
Further we need to "check other related properties:" -- within the ```nutch-default.xml``` file and accordingly update our ```nutch-site.xml```
#

```
<!-- HTTP properties -->

<property>
  <name>http.agent.name</name>
  <value></value>
  <description>HTTP 'User-Agent' request header. MUST NOT be empty - 
  please set this to a single word uniquely related to your organization.

  NOTE: You should also check other related properties:

	http.robots.agents
	http.agent.description
	http.agent.url
	http.agent.email
	http.agent.version

  and set their values appropriately.

  </description>
</property>
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
~/Nutch1$ solr/bin/solr start create_core -c demo
#
```
http://localhost:8983/solr/#/

```
#
We can see a GUI page of SOLR at the LocalHost 8983
To summarize - 
1. We have HBase running.
2. We have SOLR running.
3. We have Compiled Nutch with Ant.

Now we open DIR -- ```/home/dhankar/Nutch1/nutch/runtime/local/conf ```

```
dhankar@dhankar-VPCEB44EN:~/Nutch1/nutch/runtime/local/conf$ pwd
/home/dhankar/Nutch1/nutch/runtime/local/conf
dhankar@dhankar-VPCEB44EN:~/Nutch1/nutch/runtime/local/conf$ ls
automaton-urlfilter.txt     gora.properties               nutch-site.xml
configuration.xsl           gora-solr-host-schema.xml     parse-plugins.dtd
domain-suffixes.xml         gora-solr-mapping.xml         parse-plugins.xml
domain-suffixes.xsd         gora-solr-webpage-schema.xml  prefix-urlfilter.txt
domain-urlfilter.txt        gora-sql-mapping.xml          regex-normalize.xml
elasticsearch.conf          hbase-site.xml                regex-urlfilter.txt
gora-accumulo-mapping.xml   httpclient-auth.xml           schema.xml
gora-cassandra-mapping.xml  log4j.properties              solrindex-mapping.xml
gora-hbase-mapping.xml      nutch-conf.xsl                subcollections.xml
gora-mongodb-mapping.xml    nutch-default.xml             suffix-urlfilter.txt
dhankar@dhankar-VPCEB44EN:~/Nutch1/nutch/runtime/local/conf$ 
```
Edit the File - ```gedit regex-urlfilter.txt```

End of File - after # accept anything else - add a URL followed by a CARET ^ 
#
Create a DIR - urls

```
dhankar@dhankar-VPCEB44EN:~/Nutch1/nutch/runtime/local$ mkdir urls
dhankar@dhankar-VPCEB44EN:~/Nutch1/nutch/runtime/local$ ls
bin  conf  lib  plugins  test  urls
dhankar@dhankar-VPCEB44EN:~/Nutch1/nutch/runtime/local$ 
```

# 
When Injecting URLs DIR and the seed.txt file -- facing issues of Delays and Freezes 

While Checking Error Logs in DIR --- ```/home/dhankar/Nutch1/nutch/runtime/local/logs```
We look at file - ```hadoop.log``` , within which we search for text - "Caused by:" as an initial check ...
For instance - We dig up this line below -- 
"Caused by: org.apache.zookeeper.KeeperException$ConnectionLossException: KeeperErrorCode = ConnectionLoss for /hbase/meta-region-server"
#
For now added entries as below  --- maxClientCnxns=60 --- in the ``` /home/dhankar/Nutch1/zookeeper/conf/zoo.cfg ``` file 
But it seems the maxClientCnxns - is Not the issue . 
#
```
# the maximum number of client connections.
# increase this if you need to handle more clients
#### DHANKAR --- Added this - maxClientCnxns=60 - to the HBase-site.xml - File also - wasnt done earlier ...

maxClientCnxns=60
#
```
#
Zookeeper is facing challenges starting up / connecting with HBase ? 
#
Few quick things learnt and tried a couple of quick fixes - none seems to work as of now - documenting them below --

#### Issue -1 -- 
Not sure if Zookeeper Runs on its own even though we have chosen the HBase option where Zookeeper is managed by HBase and probaly doesnt need to be started or stopped by user manually. 

When we start Zookeeper manually with the cmd -- ```dhankar@dhankar-VPCEB44EN:~/Nutch1/zookeeper$ sudo bin/zkServer.sh start```
We get the following in console -- 
```ZooKeeper JMX enabled by default
Using config: /home/dhankar/Nutch1/zookeeper/bin/../conf/zoo.cfg
Starting zookeeper ... STARTED
```
```
## Issue -1 --- Zookepper seems Not to Read JAVA_HOME 
dhankar@dhankar-VPCEB44EN:~/Nutch1/zookeeper$ cat zookeeper.out
nohup: failed to run command '/usr/lib/jvm/java-8-oracle/jre/bin/java/bin/java': Not a directory


```
#
```
2017-06-27 19:19:17,612 [myid:] - INFO  [main:ZooKeeper@438] - Initiating client connection, connectString=localhost:2181 sessionTimeout=30000 watcher=org.apache.zookeeper.ZooKeeperMain$MyWatcher@506c589e
Welcome to ZooKeeper!
```

#
SOLR Announces its PID --- on executing - jps -- SOLR is shown as "jar"


#








