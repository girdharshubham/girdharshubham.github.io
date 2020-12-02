---  
title: "How to increase the replication factor for a Kafka topic"
comments: true
category: ["kafka"]
---  

> Originally published at the [Knoldus Blog](https://blog.knoldus.com/devops-shorts-how-to-increase-the-replication-factor-for-a-kafka-topic/){:target="_blank"}
  
Have you ever faced a situation where you had to increase the replication factor for a topic? Turns out it's really easy to do it.
In this super short blog, let's try to do just that. <br />
We'd start with creating a topic, `one`, with a replication factor of just `1` and then work on bits that include creating the `increase.json` file and then actually triggering the plan.

## Step 1: Create Kafka Topic
{% highlight shell %}
$ ./bin/kafka-topics.sh --bootstrap-server localhost:9092 --create --topic one --replication-factor 1 --partitions 10
Created topic one.
{% endhighlight %}  

## Step 2: Describe the topic created
{% highlight shell %}
$ ./bin/kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic one
Topic: one	PartitionCount: 10	ReplicationFactor: 1	Configs: segment.bytes=1073741824
	Topic: one	Partition: 0	Leader: 1	Replicas: 1	Isr: 1
	Topic: one	Partition: 1	Leader: 0	Replicas: 0	Isr: 0
	Topic: one	Partition: 2	Leader: 1	Replicas: 1	Isr: 1
	Topic: one	Partition: 3	Leader: 0	Replicas: 0	Isr: 0
	Topic: one	Partition: 4	Leader: 1	Replicas: 1	Isr: 1
	Topic: one	Partition: 5	Leader: 0	Replicas: 0	Isr: 0
	Topic: one	Partition: 6	Leader: 1	Replicas: 1	Isr: 1
	Topic: one	Partition: 7	Leader: 0	Replicas: 0	Isr: 0
	Topic: one	Partition: 8	Leader: 1	Replicas: 1	Isr: 1
	Topic: one	Partition: 9	Leader: 0	Replicas: 0	Isr: 0
{% endhighlight %}  

## Step 3: Create the json file with the topic reassignment details
{% highlight shell %}
$ cat >>increase.json <<EOF
{
 "version":1,
 "partitions":[
      {"topic":"one","partition":0,"replicas":[0,1]},
      {"topic":"one","partition":1,"replicas":[1,0]},
      {"topic":"one","partition":2,"replicas":[0,1]},
      {"topic":"one","partition":3,"replicas":[0,1]},
      {"topic":"one","partition":4,"replicas":[0,1]},
      {"topic":"one","partition":5,"replicas":[0,1]},
      {"topic":"one","partition":6,"replicas":[0,1]},
      {"topic":"one","partition":7,"replicas":[0,1]},
      {"topic":"one","partition":8,"replicas":[0,1]},
      {"topic":"one","partition":9,"replicas":[0,1]}
 ]
}
EOF
{% endhighlight %}  

## Step 4: Execute this reassignment plan
{% highlight shell %}
$ ./bin/kafka-reassign-partitions.sh --bootstrap-server localhost:9092 --reassignment-json-file increase.json --execute
Current partition replica assignment

{"version":1,"partitions":[{"topic":"one","partition":0,"replicas":[0,1],"log_dirs":["any","any"]},{"topic":"one","partition":1,"replicas":[1,0],"log_dirs":["any","any"]},{"topic":"one","partition":2,"replicas":[0,1],"log_dirs":["any","any"]},{"topic":"one","partition":3,"replicas":[1,0],"log_dirs":["any","any"]},{"topic":"one","partition":4,"replicas":[0,1],"log_dirs":["any","any"]},{"topic":"one","partition":5,"replicas":[1,0],"log_dirs":["any","any"]},{"topic":"one","partition":6,"replicas":[0,1],"log_dirs":["any","any"]},{"topic":"one","partition":7,"replicas":[1,0],"log_dirs":["any","any"]},{"topic":"one","partition":8,"replicas":[0,1],"log_dirs":["any","any"]},{"topic":"one","partition":9,"replicas":[1,0],"log_dirs":["any","any"]}]}

Save this to use as the --reassignment-json-file option during rollback
Successfully started partition reassignments for one-0,one-1,one-2,one-3,one-4,one-5,one-6,one-7,one-8,one-9
{% endhighlight %}  

## Step 5: Describe the topic again
{% highlight shell %}
$ ./bin/kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic one
Topic: one	PartitionCount: 10	ReplicationFactor: 2	Configs: segment.bytes=1073741824
	Topic: one	Partition: 0	Leader: 1	Replicas: 0,1	Isr: 1,0
	Topic: one	Partition: 1	Leader: 0	Replicas: 1,0	Isr: 0,1
	Topic: one	Partition: 2	Leader: 1	Replicas: 0,1	Isr: 1,0
	Topic: one	Partition: 3	Leader: 0	Replicas: 0,1	Isr: 0,1
	Topic: one	Partition: 4	Leader: 1	Replicas: 0,1	Isr: 1,0
	Topic: one	Partition: 5	Leader: 0	Replicas: 0,1	Isr: 0,1
	Topic: one	Partition: 6	Leader: 1	Replicas: 0,1	Isr: 1,0
	Topic: one	Partition: 7	Leader: 0	Replicas: 0,1	Isr: 0,1
	Topic: one	Partition: 8	Leader: 1	Replicas: 0,1	Isr: 1,0
	Topic: one	Partition: 9	Leader: 0	Replicas: 0,1	Isr: 0,1
{% endhighlight %}  
