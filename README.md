# secure-spark-streaming-kafka

This demo is build to get the counts of various words in every batch interval fetched from secured Kafka cluster using this Spark Streaming Application.

Note: This Demo is build for CDS(Spark) Version: 2.3.0.cloudera3 & CDK(Kafka) Version: 4.0.0. 

For kerberos enabled Kafka, we need CDS 2.1+ and CDK 2.1+ to make the combination work:
https://blog.cloudera.com/blog/2017/05/reading-data-securely-from-apache-kafka-to-apache-spark/

Requirements
Cloudera Distribution of Spark 2.1 release 1 or higher
Cloudera Distribution of Kafka 2.1.0 or higher

also they need KAFKA 0-10 integration (export SPARK_KAFKA_VERSION=0.10 before launch spark2-submit).



