---
layout: page
title: Working with Hadoop via the Command Line Starting HDFS and Yarn
permalink: /linux/distributed-systems/hadoop/starting-hdfs-and-yarn/
---


    ssh student@192.168.1.11

192.168.1.11 - ip назначенный виртуальной машине с hadoop

    $ sudo su - hadoop

<br/>

    $ hadoop version
    Hadoop 2.5.2


<br/>

    $ jps
    2250 Jps

<br/>

    $ cd /srv/hadoop/sbin/
    $ ./start-dfs.sh

<br/>

    $ jps
    2511 DataNode
    2816 Jps
    2705 SecondaryNameNode
    2394 NameNode

<br/>

    ./start-yarn.sh


<br/>


    $ jps
    2511 DataNode
    2867 ResourceManager
    2987 NodeManager
    2705 SecondaryNameNode
    2394 NameNode
    3291 Jps


<br/>

     ./mr-jobhistory-daemon.sh start historyserver

<br/>

     $ jps
     2511 DataNode
     3386 Jps
     2867 ResourceManager
     2987 NodeManager
     3324 JobHistoryServer
     2705 SecondaryNameNode
     2394 NameNode



В браузере коннекчусь к

http://192.168.1.11:50070/  
http://192.168.1.11:50090  
http://192.168.1.11:8088/
http://192.168.1.11:19888
