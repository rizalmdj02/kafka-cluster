
# Kafka Cluster configuration

Here is an example configuration for a 3-node Kafka cluster with TLS and ACL configurations. Please refer to each folder node for broker and ZooKeeper configuration.




## Create service D

In this section, we will create `systemd`  unit files for the Kafka service

- Create Zookeeper service
```
sudo vi /etc/systemd/system/zookeeper.service
```
enter this  following code
```
[Unit]
Requires=network.target remote-fs.target
After=network.target remote-fs.target

[Service]
Type=simple
User=<user for run kafka service> 
ExecStart=/<kafka-location>/bin/zookeeper-server-start.sh /<kafka-location>/config/zookeeper.properties
ExecStop=/<kafka-location>/bin/zookeeper-server-stop.sh
Restart=on-abnormal

[Install]
WantedBy=multi-user.target
```

- Create broker service

```
sudo vi /etc/systemd/system/kafka.service
```
enter this  following code
```
[Unit]
Requires=zookeeper.service
After=zookeeper.service

[Service]
Type=simple
User=kafka
ExecStart=/bin/sh -c '/home/kafka/kafka/bin/kafka-server-start.sh /home/kafka/kafka/config/server.properties > /data/kafka/broker/kafka.log 2>&1'
ExecStop=/home/kafka/kafka/bin/kafka-server-stop.sh
Restart=on-abnormal

[Install]
WantedBy=multi-user.target

```