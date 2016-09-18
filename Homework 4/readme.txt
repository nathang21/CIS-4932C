// Confirm environmental variables
export JAVA_HOME=/usr/java/default
export PATH=${JAVA_HOME}/bin:${PATH}
export HADOOP_CLASSPATH=${JAVA_HOME}/lib/tools.jar

// Ensure Hadooop is running properly will all nodes
start-all.sh

// Confirm working directory is hadoop install
cd ~/hadoop-2.2.0

// Compile Java code
bin/hadoop com.sun.tools.javac.Main WordCount.java
jar cf wc.jar WordCount*.class

// Clean-up any leftover data
hadoop namenode -format

// Format tmp directory
rm -rf tmp

// Cleanup previous Input Output
hadoop fs -rmr /input/
hadoop fs -rmr /output

// Setup input ("file" contains the text from input.txt taken from WebCourses)
hadoop fs -mkdir /input
hadoop fs -put file /input

// Run modified WordCount
bin/hadoop jar wc.jar WordCount /input /output

// Print Output to Screen
hadoop fs -cat /output/part-r-00000
