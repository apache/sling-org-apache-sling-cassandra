[![Apache Sling](https://sling.apache.org/res/logos/sling.png)](https://sling.apache.org)

&#32;[![Build Status](https://ci-builds.apache.org/job/Sling/job/modules/job/sling-org-apache-sling-cassandra/job/master/badge/icon)](https://ci-builds.apache.org/job/Sling/job/modules/job/sling-org-apache-sling-cassandra/job/master/)&#32;[![Test Status](https://img.shields.io/jenkins/tests.svg?jobUrl=https://ci-builds.apache.org/job/Sling/job/modules/job/sling-org-apache-sling-cassandra/job/master/)](https://ci-builds.apache.org/job/Sling/job/modules/job/sling-org-apache-sling-cassandra/job/master/test/?width=800&height=600)&#32;[![Sonarcloud Status](https://sonarcloud.io/api/project_badges/measure?project=apache_sling-org-apache-sling-cassandra&metric=alert_status)](https://sonarcloud.io/dashboard?id=apache_sling-org-apache-sling-cassandra)&#32;[![Contrib](https://sling.apache.org/badges/status-contrib.svg)](https://github.com/apache/sling-aggregator/blob/master/docs/status/contrib.md) [![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://www.apache.org/licenses/LICENSE-2.0)

# Apache Sling Cassandra Resource Provider

This module is part of the [Apache Sling](https://sling.apache.org) project.

Initially contributed by Dishara Wijewardana as part of GSoC 2013.

This doc will guide on how to setup Cassandra resource provider with 
Sling trunk.

- Pre requisits
   You should have maven 3.
   Start Cassandra server in back ground.
   An up and running Sling launchpad server (build from trunk).

How To:  
-  Build the source code with Maven 3.x
-  Find the jar file inside the target folder.
-  Go to OSGi GUI console provided by Sling and install a new OSGi bundle using the above jar.
-  Now you will see the bundle is successfully installed.

Now Cassandra Resource Provider is up and running inside sling container.

- Test

Under src/test/java/org/apache/sling/cassandra/test/data/populator, you will find several sample 
tests which can run against the running instance. There are tests that includes 
Adding/Deleting/Updating/Creating new cassandra resources in sling. And performance tests 
under perf package which creates you latency reports on the test runs. Also the newly added 
utillity class which is AccessControlUtil which evaluates the priviledges for a user at a 
given path (this is just a executable utility class which is not yet in cooperated to proper 
interfaces).

Following is a sample code block of Cassandra Resource Provider 

CREATE CODE

            String path1 ="/content/cassandra/movies/xmen";

            CassandraResourceProvider cassandraResourceProvider = new CassandraResourceProvider();
            createColumnFamily("movies", cassandraResourceProvider.getKeyspace(), new StringSerializer());
            cassandraResourceProvider.setColumnFamily("movies");

            Map<String,Object> map1 = new HashMap<String, Object>();
            map1.put("metadata", "resolutionPathInfo=json");
            map1.put("resourceType", "nt:cassandra0");
            map1.put("resourceSuperType", "nt:supercass1");

            CassandraResourceResolver resolver =  new CassandraResourceResolver();
            cassandraResourceProvider.create(resolver,path1,map1);
            cassandraResourceProvider.commit(resolver);


TO READ
HTTP call to http://localhost:8080/content/cassandra/movies/xmen will return you the json view of it as 
we already specified the path to resolve as a json.

To DELETE
            cassandraResourceProvider.delete(resolver,"/content/cassandra/movies/xmen");
            cassandraResourceProvider.commit(resolver);



 




    
