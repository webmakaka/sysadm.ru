---
layout: page
title: Working with Hadoop via the Command Line: Loading Data into HDFS
permalink: /linux/distributed-systems/hadoop/loading-data-into-hdfs/
---

    $ hdfs namenode -format
    $ exit


<br/>

    $ cd ~
    $ wget http://norvig.com/ngrams/shakespeare.txt ~/
    $ hadoop fs -mkdir -p shakespeare/input
    $ hadoop fs -copyFromLocal ~/shakespeare.txt shakespeare/input
    $ hadoop fs -ls shakespeare/input
