---
title: kafka add replica
date: 2017-02-21T23:37:34.000Z
tags: kafka
---

Sometimes it is inevitable we want to change the replicas of an existent topic.

Here is steps

1. Create a JSON file named increase-replication-factor.json with the following code:

  ```json
  {
      "version": 1,
      "partitions": [{
          "topic": "dev_tracer",
          "partition": 0,
          "replicas": [0, 1, 2]
      }]
  }
  ```

2. Run the following command:

  ```shell
  > bin/kafka-reassign-partitions.sh --zookeeper localhost:2181 --reassignment-json-file increase-replication-factor.json --execute
  ```
