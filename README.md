Overview
=========

The Solr Full-Text Index provider is a new addition to the set of supported indices backing the [*Titan Graph Database*](http://thinkaurelius.github.io/titan/).  Solr is a full-fledged Apache project that uses the Lucene indexing engine and is comparable to ElasticSearch. Solr is a highly performant, and tunable index. One of the biggest differences between Solr and ElasticSearch has to do with how data types are defined in their respective settings. Unlike ElasticSearch, Solr requires you to define a schema in a file called schema.xml, before you can create the index. Though this may have some run-time disadvantages to ElasticSearch, Solr has fined grained control over the data types within its schema. Solr can run in 3 different modes: 
* __Embedded Mode__ - Executes Solr within the same JVM. The provider is coded to support this but this feature has not been extensively testsed.
* __HTTP Solr Server__ - This has been the traditional mode of Solr execution until version 4.0 was released. It runs Solr as a java web application supporting the Solr Query API using RESTful style queries or the Java based SolrJ API.
* __Cloud Mode__ - Since version 4.0 was released, Solr has added capabilities to run in a cluster or "cloud" with the ability to replicate Solr indices (or cores) across nodes in a cluster. It accomplishes this by leveraging Apache Zookeeper to distribute configuration and enable participating nodes to replicate index data. The advantage of this capability is that it adds redundancy and greater availability of Solr to applications and is encouraged for enterprise deployments.

Supported Modes
===============

The Solr Provider has been tested on HTTP Solr Server and Cloud Mode. While the provider is capable of connecting to Solr in Embedded mode, this feature has not be heavily tested.  Example code can be found in the source for Titan on the [current fork](https://github.com/tenaciousjzh/titan) where the Solr Provider code is hosted. To run the Titan integration tests with HTTP Solr Server, see the section titled **Set Up HTTP Solr Server** for more details. For more information on running tests on Solr Cloud, checkout the section titled **Set Up Solr Cloud**.

Step 1: Clone the Git Repo
=======

Clone the repo to your development environment by running the git command:
```sh
git clone git@github.com:tenaciousjzh/titan-solr-cloud-test.git
cd titan-solr-cloud-test
```

Step 2: Run Solr
================

Based on the mode you wish to execute, read the instructions under the following subheadings. In either case, the repo has been configured to use the Solr schemas needed to support the integration tests.

Option 1: Set Up HTTP Solr Server
-----------------------
Running HTTP Solr Server is the most straightforward option and is a good choice for development. Since the APIs between HTTP Solr Server and Solr Cloud are the same, your code should work the same. To set up the HTTP Solr Server:
1. Make sure you've cloned the git repo and changed directories into the solr-4.4.0 example folder and list the directory.
```bash
cd titan-solr-cloud-test/example
ls -la
total 196
drwxr-xr-x 18 jholmberg jholmberg  4096 Sep  7 11:34 .
drwxr-xr-x  8 jholmberg jholmberg  4096 Sep  7 11:34 ..
-rw-------  1 jholmberg jholmberg    59 Sep  7 11:34 .directory
-rwxr-xr-x  1 jholmberg jholmberg   101 Sep  7 11:34 1-upload_collection1_to_zk.sh
-rwxr-xr-x  1 jholmberg jholmberg    91 Sep  7 11:34 1-upload_store1_to_zk.sh
-rwxr-xr-x  1 jholmberg jholmberg    91 Sep  7 11:34 1-upload_store2_to_zk.sh
-rwxr-xr-x  1 jholmberg jholmberg    91 Sep  7 11:34 1-upload_store3_to_zk.sh
-rwxr-xr-x  1 jholmberg jholmberg    92 Sep  7 11:34 1-upload_store_to_zk.sh
-rwxr-xr-x  1 jholmberg jholmberg   123 Sep  7 11:34 2-link_collection1_to_zk.sh
-rwxr-xr-x  1 jholmberg jholmberg   113 Sep  7 11:34 2-link_store1_to_zk.sh
-rwxr-xr-x  1 jholmberg jholmberg   113 Sep  7 11:34 2-link_store2_to_zk.sh
-rwxr-xr-x  1 jholmberg jholmberg   113 Sep  7 11:34 2-link_store3_to_zk.sh
-rwxr-xr-x  1 jholmberg jholmberg   114 Sep  7 11:34 2-link_store_to_zk.sh
-rwxr-xr-x  1 jholmberg jholmberg    76 Sep  7 11:34 3-boostrap_store.sh
-rw-r--r--  1 jholmberg jholmberg  2992 Sep  7 11:34 README.txt
drwxr-xr-x  4 jholmberg jholmberg  4096 Sep  7 11:34 SOLR-INDEXING
drwxr-xr-x  4 jholmberg jholmberg  4096 Sep  7 11:34 back-up-for-webapps
drwxr-xr-x  2 jholmberg jholmberg  4096 Sep  7 11:34 cloud-scripts
drwxr-xr-x  2 jholmberg jholmberg  4096 Sep  7 11:34 contexts
drwxr-xr-x  2 jholmberg jholmberg  4096 Sep  7 11:34 etc
drwxr-xr-x  4 jholmberg jholmberg  4096 Sep  7 11:34 example-DIH
drwxr-xr-x  3 jholmberg jholmberg  4096 Sep  7 11:34 example-schemaless
drwxr-xr-x  2 jholmberg jholmberg  4096 Sep  7 11:34 exampledocs
-rw-r--r--  1 jholmberg jholmberg   208 Sep  7 11:34 input1.csv
drwxr-xr-x  3 jholmberg jholmberg  4096 Sep  7 11:34 lib
drwxr-xr-x  2 jholmberg jholmberg  4096 Sep  7 11:34 logs
drwxr-xr-x  5 jholmberg jholmberg  4096 Sep  7 11:34 multicore
drwxr-xr-x  2 jholmberg jholmberg  4096 Sep  7 11:34 resources
drwxr-xr-x  8 jholmberg jholmberg  4096 Sep  7 11:34 solr
-rwxr-xr-x  1 jholmberg jholmberg   143 Sep  7 11:34 solr-titan-cloud1.sh
-rwxr-xr-x  1 jholmberg jholmberg    72 Sep  7 11:34 solr-titan-cloud2.sh
drwxr-xr-x  6 jholmberg jholmberg  4096 Sep  7 11:34 solr-titan-test
-rwx------  1 jholmberg jholmberg   212 Sep  7 11:34 solr-titan-test.sh
drwxr-xr-x  3 jholmberg jholmberg  4096 Sep  7 11:34 solr-webapp
-rw-r--r--  1 jholmberg jholmberg 46294 Sep  7 11:34 start.jar
-rwxr-xr-x  1 jholmberg jholmberg    70 Sep  7 11:34 start_master.sh
-rwxr-xr-x  1 jholmberg jholmberg    75 Sep  7 11:34 start_replica.sh
drwxr-xr-x  2 jholmberg jholmberg  4096 Sep  7 11:34 webapps
```
1. The titan based integration tests assume that the Full-Text Index and its corresponding provider support geospatial queries. Solr supports geospatial but requires that additional libraries be added to the class path in Java at run time in order to work correctly. The version of solr that is in this project has been modified to include the appropriate Geospatial libraries for queries.  Also, it is assumed you have Java 6 or higher installed and its home directory in your path so that the **java** command can be run from the shell. To start in HTTP mode, run the following script at the shelL.
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      
```bash
./run_solr_http.sh
```
You should see the embedded Jetty client start up and log to the console.
1. You can make sure Solr is running by going to the Solr Admin page at [http://localhost:8983/solr/#/](http://localhost:8983/solr/#/). It should look something like this:

![A Screenshot of the Solr Admin Page](/media/solr_admin_screenshot.png "A Screenshot of the Solr Admin Page")

Option 2: Set Up Solr Cloud
-----------------
Solr Cloud has a few more moving parts than the standard HTTP Solr Server since its geared for running a cluster of machines and doing replication between nodes to allow you to spread index storage across them. Regardless of the size of the cluster you choose to create, Solr Cloud requires at least one instance of Apache Zookeeper to be running. Solr 4.4.0 ships with an embedded version of Zookeeper. In this project, we ship a separate instance as this is just as easy to set up and aligns more closely with a production deployment. 

This project has been set up to run a single instance of Zookeeper with 2 Solr nodes replicating 4 indexes between them. 

The index names in this project mirror the ones referred to IndexProviderTest class found in the the titan source code under the titan-test project. 

You can find the indexes by listing the following directory from the root of the project:

```bash
cd solr-4.4.0/example/solr
ls -la
total 40
drwxr-xr-x  8 jholmberg jholmberg 4096 Sep  7 11:34 .
drwxr-xr-x 18 jholmberg jholmberg 4096 Sep  7 15:37 ..
-rw-r--r--  1 jholmberg jholmberg 2473 Sep  7 11:34 README.txt
drwxr-xr-x  2 jholmberg jholmberg 4096 Sep  7 11:34 bin
drwxr-xr-x  4 jholmberg jholmberg 4096 Sep  7 11:34 collection1
-rw-r--r--  1 jholmberg jholmberg 1754 Sep  7 11:34 solr.xml
drwxr-xr-x  4 jholmberg jholmberg 4096 Sep  7 11:34 store
drwxr-xr-x  4 jholmberg jholmberg 4096 Sep  7 11:34 store1
drwxr-xr-x  4 jholmberg jholmberg 4096 Sep  7 11:34 store2
drwxr-xr-x  4 jholmberg jholmberg 4096 Sep  7 11:34 store3
```
Each index (or core as Solr calls them) reside in their own directory in store, store1, store2, and store3, respectively.

Okay, let's start up Zookeeper first since that's where Solr will look to get its configuration info. The version of Zookeeper that ships with this project has been preloaded with the appropriate configs to work out of the box.

From the root directory of the project, change directories into zookeeper-3.3.5 and run the zkServer.sh script:

```bash
cd zookeeper-3.3.5
bin/zkServer.sh start
JMX enabled by default
Using config: /home/jholmberg/Projects/titan-solr-cloud-test/zookeeper-3.3.5/bin/../conf/zoo.cfg
Starting zookeeper ... STARTED
```
If everything worked, you should see the last line of output say "Starting zookeeper ... STARTED" like the output above.
