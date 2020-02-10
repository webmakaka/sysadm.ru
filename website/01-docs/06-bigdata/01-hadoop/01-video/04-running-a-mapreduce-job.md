---
layout: page
title: Working with Hadoop via the Command Line Running a MapReduce Job
permalink: /bigdata/hadoop/running-a-mapreduce-job/
---

# Working with Hadoop via the Command Line Running a MapReduce Job

    $ cd /srv/hadoop/share/hadoop/mapreduce/


<br/>

    $ hadoop jar hadoop-mapreduce-examples-2.5.2.jar wordcount shakespeare/input shakespeare/output
    Error creating temp dir in hadoop.tmp.dir /var/app/hadoop/data due to Permission denied


<br/>

    $ sudo chmod -R 771 /var/app/hadoop/

<br/>

    $ ls -la /var/app/hadoop/
    drwxrwx--x 4 hadoop hadoop 4096 Jan  7  2015 data

<br/>  


    $ hadoop jar hadoop-mapreduce-examples-2.5.2.jar wordcount shakespeare/input shakespeare/output

    15/07/25 09:41:36 INFO client.RMProxy: Connecting to ResourceManager at localhost/127.0.0.1:8050
    org.apache.hadoop.security.AccessControlException: Permission denied: user=student, access=EXECUTE, inode="/tmp":hadoop:supergroup:drwxrwx---


<br/>

    $ hadoop fs -ls /tmp

    ls: Permission denied: user=student, access=READ_EXECUTE, inode="/tmp":hadoop:supergroup:drwxrwx---


<br/>


    $ sudo su - hadoop

Прав 771 не хватило

    $ hadoop fs -chmod -R 777 /tmp
    $ exit

<br/>

    $ hadoop jar hadoop-mapreduce-examples-2.5.2.jar wordcount shakespeare/input shakespeare/output


    15/07/25 09:49:44 INFO client.RMProxy: Connecting to ResourceManager at localhost/127.0.0.1:8050
    15/07/25 09:49:44 INFO input.FileInputFormat: Total input paths to process : 1
    15/07/25 09:49:45 INFO mapreduce.JobSubmitter: number of splits:1
    15/07/25 09:49:46 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1437841736026_0001
    15/07/25 09:49:47 INFO impl.YarnClientImpl: Submitted application application_1437841736026_0001
    15/07/25 09:49:47 INFO mapreduce.Job: The url to track the job: http://hadoop:8088/proxy/application_1437841736026_0001/
    15/07/25 09:49:47 INFO mapreduce.Job: Running job: job_1437841736026_0001
    15/07/25 09:49:59 INFO mapreduce.Job: Job job_1437841736026_0001 running in uber mode : false
    15/07/25 09:49:59 INFO mapreduce.Job:  map 0% reduce 0%
    15/07/25 09:50:10 INFO mapreduce.Job:  map 100% reduce 0%
    15/07/25 09:50:20 INFO mapreduce.Job:  map 100% reduce 100%
    15/07/25 09:50:20 INFO mapreduce.Job: Job job_1437841736026_0001 completed successfully
    15/07/25 09:50:20 INFO mapreduce.Job: Counters: 49
    	File System Counters
    		FILE: Number of bytes read=483860
    		FILE: Number of bytes written=1161747
    		FILE: Number of read operations=0
    		FILE: Number of large read operations=0
    		FILE: Number of write operations=0
    		HDFS: Number of bytes read=4538656
    		HDFS: Number of bytes written=356409
    		HDFS: Number of read operations=6
    		HDFS: Number of large read operations=0
    		HDFS: Number of write operations=2
    	Job Counters
    		Launched map tasks=1
    		Launched reduce tasks=1
    		Data-local map tasks=1
    		Total time spent by all maps in occupied slots (ms)=8538
    		Total time spent by all reduces in occupied slots (ms)=7626
    		Total time spent by all map tasks (ms)=8538
    		Total time spent by all reduce tasks (ms)=7626
    		Total vcore-seconds taken by all map tasks=8538
    		Total vcore-seconds taken by all reduce tasks=7626
    		Total megabyte-seconds taken by all map tasks=8742912
    		Total megabyte-seconds taken by all reduce tasks=7809024
    	Map-Reduce Framework
    		Map input records=129107
    		Map output records=980637
    		Map output bytes=8406347
    		Map output materialized bytes=483860
    		Input split bytes=133
    		Combine input records=980637
    		Combine output records=33505
    		Reduce input groups=33505
    		Reduce shuffle bytes=483860
    		Reduce input records=33505
    		Reduce output records=33505
    		Spilled Records=67010
    		Shuffled Maps =1
    		Failed Shuffles=0
    		Merged Map outputs=1
    		GC time elapsed (ms)=144
    		CPU time spent (ms)=4890
    		Physical memory (bytes) snapshot=346832896
    		Virtual memory (bytes) snapshot=1600458752
    		Total committed heap usage (bytes)=222101504
    	Shuffle Errors
    		BAD_ID=0
    		CONNECTION=0
    		IO_ERROR=0
    		WRONG_LENGTH=0
    		WRONG_MAP=0
    		WRONG_REDUCE=0
    	File Input Format Counters
    		Bytes Read=4538523
    	File Output Format Counters
    		Bytes Written=356409



http://192.168.1.11:8088/

Появилось какое-то приложение.


    $ hadoop fs -ls shakespeare/output

    Found 2 items
    -rw-r--r--   1 student student          0 2015-07-25 09:50 shakespeare/output/_SUCCESS
    -rw-r--r--   1 student student     356409 2015-07-25 09:50 shakespeare/output/part-r-00000

<br/>

    $ hadoop fs -cat shakespeare/output/part-r-00000


<br/>

    $ hadoop fs -copyToLocal shakespeare/output/part-r-00000 ~/shakespeare_output.txt

<br/>

    $ cd ~/
    $ tail -n 20 shakespeare_output.txt

<br/>

    $ hadoop fs -getmerge shakespeare/output ~/shakespeare_output.txt
