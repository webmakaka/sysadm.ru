---
layout: page
title: Loading Data into HDFS
description: Loading Data into HDFS
keywords: Loading Data into HDFS
permalink: /bigdata/hadoop/loading-data-into-hdfs/
---

# Loading Data into HDFS

user hadoop

    $ hdfs namenode -format

Re-format filesystem in Storage Directory /var/app/hadoop/data/dfs/name [Y]

    $ exit

user student

    $ cd ~
    $ wget http://norvig.com/ngrams/shakespeare.txt ~/
    $ hadoop fs -mkdir -p shakespeare/input
    $ hadoop fs -copyFromLocal ~/shakespeare.txt shakespeare/input
    $ hadoop fs -ls shakespeare/input
