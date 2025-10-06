Awesome 🚀 — we’ll go with **Option 1: Full Markdown version + image/diagram placeholders**, perfectly formatted for **Medium or GitHub**.

Here’s your **final Medium-ready blog in Markdown (`.md`)** format 👇

---

````markdown
# 🧩 Kafka Multinode Installation & KRaft Setup on Java 11 (Step-by-Step Guide)

Apache Kafka has revolutionized real-time data streaming — but with the introduction of **KRaft mode (Kafka Raft Metadata mode)**, it’s become simpler and more reliable by removing the dependency on ZooKeeper.  

In this guide, we’ll walk through **how to install and configure a multi-node Kafka cluster using KRaft mode** on **Java 11** — from scratch.

---

## 🖼️ Architecture Overview

![Kafka KRaft Architecture Diagram](https://via.placeholder.com/900x400?text=Kafka+KRaft+Architecture+Diagram)

**Explanation:**
- Each Kafka broker acts as both a **controller** and a **broker**.
- **KRaft mode** manages cluster metadata internally — no need for ZooKeeper.
- Communication happens through **controller and broker listeners** across multiple machines.

---

## 🚀 Step 1: Install Java 11

Kafka requires Java to run. We’ll use **OpenJDK 11**, which is fully compatible with Kafka 3.x.

```bash
sudo apt update
sudo apt install openjdk-11-jdk -y
java -version
````

If multiple Java versions exist, ensure **Java 11** is active:

```bash
sudo update-alternatives --config java
```

✅ Expected output:

```
openjdk version "11.x.x"
```

---

## 📦 Step 2: Download & Extract Kafka

Let’s download and set up **Kafka 3.1.0** (you can use any compatible version):

```bash
wget https://downloads.apache.org/kafka/3.1.0/kafka_2.13-3.1.0.tgz
tar -xzf kafka_2.13-3.1.0.tgz
mv kafka_2.13-3.1.0 ~/kafka
cd ~/kafka
```

![Kafka Installation Illustration](https://via.placeholder.com/900x400?text=Kafka+Installation+Process)

---

## 🌐 Step 3: Configure Hostnames on Both Machines

For a multi-node cluster, each machine must recognize the other’s hostname.

Edit the `/etc/hosts` file:

```bash
sudo nano /etc/hosts
```

Default:

```
127.0.0.1 localhost
127.0.1.1 hostname
```

Add your machine IPs:

```
172.20.xx.xx   iiita
172.29.xx.xx   hostname
```

Get your system IP:

```bash
ip addr show
```

Sample output:

```
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP>
    inet 192.168.1.10/24 brd 192.168.1.255 scope global dynamic enp0s3
```

👉 Here, `192.168.1.10` is your IP address.

---

## ⚙️ Step 4: Configure Kafka for KRaft Mode

Open the KRaft configuration file:

```bash
nano config/kraft/server.properties
```

Add or update the following:

```properties
############################# Server Basics #############################
node.id=1                 # Change to 2 on the second machine

############################# KRaft Controller #############################
controller.quorum.voters=1@hostname1:9093,2@hostname2:9093

############################# Listeners #############################
listeners=PLAINTEXT://hostname:9092,CONTROLLER://hostname:9093
listener.security.protocol.map=CONTROLLER:PLAINTEXT,PLAINTEXT:PLAINTEXT
inter.broker.listener.name=PLAINTEXT
advertised.listeners=PLAINTEXT://hostname:9092

############################# Log Directories #############################
log.dirs=/home/<USER>/kafka/kraft-combined-logs
metadata.log.dir=/home/<USER>/kafka/kraft-metadata-logs

############################# KRaft Mode #############################
process.roles=broker,controller
controller.listener.names=CONTROLLER
num.partitions=6

############################# Internal Topics #############################
offsets.topic.replication.factor=2
transaction.state.log.replication.factor=2
transaction.state.log.min.isr=1
```

> 💡 Replace `<USER>` with your system username and `hostname` with your machine’s name.
> Each node has a unique `node.id` but shares the same `controller.quorum.voters` setup.

Save and exit (`Ctrl + X`, then `Y`).

---

## 💾 Step 5: Format Log Directories & Initialize the Cluster

Before starting Kafka, clear and format log directories:

```bash
rm -rf /home/user/kafka/kraft-combined-logs/*
rm -rf /home/user/kafka/kraft-metadata-logs/*
```

Generate a **Cluster ID**:

```bash
bin/kafka-storage.sh random-uuid
```

Example output:

```
c0a8e8e4-0f2e-4d33-9b2a-7c2e3cf2c1f5
```

Format Kafka logs:

```bash
bin/kafka-storage.sh format -t <CLUSTER_ID> -c config/kraft/server.properties
```

> ⚠️ Use the **same Cluster ID** across all Kafka nodes.

---

## 🧩 Step 6: Start Kafka in KRaft Mode

Run the following command to start Kafka:

```bash
bin/kafka-server-start.sh config/kraft/server.properties
```

If everything’s configured correctly, Kafka will start **without ZooKeeper**, fully operating in **KRaft mode** 🎉.

---

## ⚡ Step 7: Verify the Kafka Setup

### ✅ Create a Topic

```bash
bin/kafka-topics.sh --create --topic test-topic --bootstrap-server hostname:9092 --partitions 1 --replication-factor 1
```

### ✅ List Topics

```bash
bin/kafka-topics.sh --list --bootstrap-server hostname:9092
```

### ✅ Start a Producer

```bash
bin/kafka-console-producer.sh --topic test-topic --bootstrap-server hostname:9092
```

### ✅ Start a Consumer (on another node)

```bash
bin/kafka-console-consumer.sh --topic test-topic --from-beginning --bootstrap-server hostname:9092
```

Now, type a few messages in the producer terminal — they should appear instantly in the consumer terminal! 💬

Check running processes:

```bash
jps
```

Example output:

```
8804 Kafka
10076 ConsoleProducer
10413 ConsoleConsumer
10745 Jps
```

---

## 🧠 Verification Workflow Diagram

![Kafka Multinode Verification Flow](https://via.placeholder.com/900x400?text=Kafka+Producer-Consumer+Flow)

---

## 🏁 Conclusion

You’ve now set up a **multi-node Kafka cluster using KRaft mode on Java 11** — without ZooKeeper! 🏆

This setup is ready for:

* Real-time streaming pipelines
* Event-driven microservices
* Data lake ingestion
* Real-time analytics

KRaft mode makes Kafka’s architecture simpler and more robust — a huge leap toward a fully self-managed distributed system.

---

## 💡 Pro Tip

If you’re planning to integrate **Apache Spark** or **Flink** for real-time processing, you can reuse this same KRaft-based Kafka setup seamlessly.

---

### ✍️ Author

**Neeraj Kumar Kannoujiya**

---


```

---

Would you like me to **generate this as a downloadable `.md` file** (so you can upload it directly to Medium or GitHub)?  
I can include a **cover image** suggestion too.
```
