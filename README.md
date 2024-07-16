# Distributed Data Storage with Hadoop HDFS and Amazon S3

## Introduction

This project demonstrates how to set up a Hadoop HDFS cluster for distributed file storage and integrate it with Amazon S3 to leverage scalable cloud storage. The project includes steps for setting up Hadoop, configuring it to work with S3, and performing file operations.

## Objectives

1. Set up a Hadoop HDFS cluster for distributed file storage.
2. Integrate HDFS with Amazon S3 for scalable cloud storage.
3. Perform file operations in both HDFS and S3.
4. Understand the architecture and components of HDFS and S3.

## Prerequisites

- Java (JDK 1.8 or later)
- Hadoop (version 3.4.0)
- AWS CLI
- AWS S3 bucket
- Internet connection

## Tools and Technologies

- Hadoop HDFS
- Amazon S3
- AWS CLI (Command Line Interface)
- Hadoop Ecosystem Tools (HDFS Shell Commands)

## Step-by-Step Implementation

### Step 1: Set Up Hadoop HDFS Cluster

1. **Install Hadoop:**
   Download Hadoop from the [official Apache Hadoop website](https://hadoop.apache.org/releases.html) and follow the installation guide for your operating system.

2. **Configure Hadoop:**
   Edit the Hadoop configuration files (`core-site.xml`, `hdfs-site.xml`) to set up the HDFS cluster.

   - `core-site.xml`:
     ```xml
     <configuration>
         <property>
             <name>fs.defaultFS</name>
             <value>hdfs://localhost:9000</value>
         </property>
         <property>
             <name>fs.s3a.access.key</name>
             <value>YourAccessKey</value>
         </property>
         <property>
             <name>fs.s3a.secret.key</name>
             <value>YourSecretKey</value>
         </property>
         <property>
             <name>fs.s3a.endpoint</name>
             <value>s3.amazonaws.com</value>
         </property>
         <property>
             <name>fs.s3a.impl</name>
             <value>org.apache.hadoop.fs.s3a.S3AFileSystem</value>
         </property>
     </configuration>
     ```

   - `hdfs-site.xml`:
     ```xml
     <configuration>
         <property>
             <name>dfs.replication</name>
             <value>1</value>
         </property>
         <property>
             <name>dfs.name.dir</name>
             <value>file:///usr/local/hadoop/hdfs/namenode</value>
         </property>
         <property>
             <name>dfs.data.dir</name>
             <value>file:///usr/local/hadoop/hdfs/datanode</value>
         </property>
     </configuration>
     ```

3. **Start HDFS:**
   Format the namenode and start HDFS services:
   ```sh
   hdfs namenode -format
   start-dfs.sh
   ```

### Step 2: Integrate HDFS with Amazon S3

1. **Install AWS CLI:**
   Follow the installation guide for the [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html).

2. **Configure AWS CLI:**
   Configure AWS CLI with your AWS credentials:
   ```sh
   aws configure
   ```

3. **Set Up S3 Bucket:**
   Create an S3 bucket using the AWS Management Console or CLI:
   ```sh
   aws s3 mb s3://your-bucket-name
   ```

4. **Transfer Data Between HDFS and S3:**
   - Copy a file from HDFS to S3:
     ```sh
     hdfs dfs -cp /user/sowrabh/hadoopfiles/input.txt s3a://distributed-data-storage/Input-files/
     ```

   - Copy a file from S3 to HDFS:
     ```sh
     hdfs dfs -cp s3a://distributed-data-storage/Input-files/input.txt /user/sowrabh/hadoopfiles/
     ```

### Step 3: Perform File Operations

1. **HDFS File Operations:**
   - List files in HDFS:
     ```sh
     hdfs dfs -ls /
     ```
   - Create a directory in HDFS:
     ```sh
     hdfs dfs -mkdir /user/yourname
     ```
   - Upload a file to HDFS:
     ```sh
     hdfs dfs -put localfile.txt /user/yourname/
     ```
   - Download a file from HDFS:
     ```sh
     hdfs dfs -get /user/yourname/localfile.txt localfile.txt
     ```

2. **S3 File Operations:**
   - List files in S3:
     ```sh
     aws s3 ls s3://your-bucket-name/
     ```
   - Upload a file to S3:
     ```sh
     aws s3 cp localfile.txt s3://your-bucket-name/localfile.txt
     ```
   - Download a file from S3:
     ```sh
     aws s3 cp s3://your-bucket-name/localfile.txt localfile.txt
     ```

### Troubleshooting

- **ClassNotFoundException:**
  Ensure that `hadoop-aws-3.4.0.jar` and `aws-java-sdk-bundle-1.11.1026.jar` are placed in the `$HADOOP_HOME/share/hadoop/tools/lib/` directory.
  Add the following to `hadoop-env.sh`:
  ```sh
  export HADOOP_CLASSPATH=$HADOOP_CLASSPATH:$HADOOP_HOME/share/hadoop/tools/lib/*
  ```

### Conclusion

This project demonstrates how to set up a distributed data storage system using Hadoop HDFS and integrate it with Amazon S3 for scalable cloud storage. By following these steps, you can efficiently manage and transfer large volumes of data between on-premise and cloud storage solutions.

### References

- [Apache Hadoop](https://hadoop.apache.org/)
- [Amazon S3](https://aws.amazon.com/s3/)
- [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
