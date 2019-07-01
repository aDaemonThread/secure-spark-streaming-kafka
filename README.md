# secure-spark-streaming-kafka

This demo is build to get the counts of various words in every batch interval fetched from secured Kafka cluster using this Spark Streaming Application.

Note: This Demo is build for CDS(Spark) Version: 2.3.0.cloudera3 & CDK(Kafka) Version: 4.0.0. 

For kerberos enabled Kafka, we need CDS 2.1+ and CDK 2.1+ to make the combination work:
https://blog.cloudera.com/blog/2017/05/reading-data-securely-from-apache-kafka-to-apache-spark/

Requirements
Cloudera Distribution of Spark 2.1 release 1 or higher
Cloudera Distribution of Kafka 2.1.0 or higher

also they need KAFKA 0-10 integration (export SPARK_KAFKA_VERSION=0.10 before launch spark2-submit).

Please follow the mentioned steps to build and execute this sample Spark Streaming Application to interact with secure Kafka cluster.

Download & Build the Demo App:

1. Download or Clone the Git repo available here Demo App GitHub Link.

2. Build this application using maven.

$ cd secure-spark-streaming-kafka

$ mvn clean package

3. If your application is built successfully, you can observe the mentioned output.

<pre>

[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 24.061 s
</pre>

4. After building your application, take the generated uber jar from target/secure-spark-streaming-kafka-1.0-SNAPSHOT-jar-with-dependencies.jar to the spark client node(where you are going to launch the query from).

Create the required file & permission:

5. Once you have the uber jar available in the client node, the next step is to create a jaas.conf file for JAAS configuration for Kerberos access. As per Apache Kafka Documentation, Kafka uses the Java Authentication and Authorization Service (JAAS) for SASL configuration.

Sample jaas.conf:

KafkaClient {
    com.sun.security.auth.module.Krb5LoginModule required
    useKeyTab=true
    storeKey=true
    keyTab="./user.kafka.keytab"
    useTicketCache=false
    serviceName="kafka"
    principal="user@MY.DOMAIN.COM";
};

Modify keytab & principal value to your actual value. KafkaServer is the section name in the JAAS file used by each KafkaServer/Broker. This section provides SASL configuration options for the broker including any SASL client connections made by the broker for inter-broker communication.

6. Also, check the permission on the created jaas.conf and keytab files to avoid file permission exceptions.

Execute the Spark Application:

7. Once all the above steps are completed successfully, we have to execute the spark2-submit command.

 SPARK_KAFKA_VERSION=0.10 spark2-submit \
--num-executors 2 \ 
--master yarn \ 
--deploy-mode client \ 
--files /<path>/jaas.conf,/<path>/user.kafka.keytab \ 
--driver-java-options "-Djava.security.auth.login.config=./jaas.conf" \ 
--class com.github.ankit.spark.example.DirectKafkaWordCount \ 
--conf "spark.executor.extraJavaOptions=-Djava.security.auth.login.config=./jaas.conf" \ 
/<path>/kafka-spark-secure-demo-1.0-SNAPSHOT-jar-with-dependencies.jar \ 
<broker_hostname>:9092 \ 
<topic_name> \ 
false
  
Note: Please replace the below-mentioned parameters with actual value.

<broker_hostname> :: Broker hostname.
<path> :: actual file path.
Modify the last parameter to “true” if you have SSL enabled and change the port accordingly.

8. As your Spark streaming application is running now, in another terminal, start a producer that publishes to a topic using “kafka-console-producer” client.

9. You can observe the counts of various words in every batch interval, in your spark streaming driver’s stdout.




