---
layout: post
title:  "Apache Kafka"
summary: "What is Apache Kafka?"
author: seunghwan
date: '2024-03-07 00:00:00 +0530'
category: ['Apache Spark']
tags: Pyspark
usemathjax: false
permalink: /blog/Apache-Kafka/
---

## Apache Kafka란?
Apache Kafka는 시작 또는 끝이 없는 스트리밍 이벤트 데이터나 일반 데이터를 수집, 처리, 저장하는데 사용되는 분산 메세지 스트리밍 플랫폼입니다. 대용량 실시간 로그처리에 특화된만큼 Fault-Tolerant 한 안정적인 아키텍처를 제공하며, 분산 애플리케이션 확장을 통해 스트리밍 이벤트를 분당 수십억 개까지 처리할 수 있고 필요한 모든곳에서 대규모 데이터를 동시에 이동할 수 있습니다. 이 외에도 스트리밍 데이터를 가져와 정확히 언제 무슨 일이 일어났는지 기록합니다. 이 기록은 변경 불가능한 커밋 로그라고 합니다. 즉 추가는 가능하지만 변경은 불가능합니다. 로그에 있는 데이터에 액세스할 수 있으며, 개수 제한 없이 여러 스트리밍 실시간 애플리케이션과 다른 시스템에서 로그에 데이터를 추가할 수도 있습니다.

* Producer/Consumber와 Publish/Subscribe을 같은 의미로 사용하고 있었습니다

## Kafka 설치

### Oracle Linux 8.6

```bash
# 다운로드
$ dnf install java-17-openjdk java-17-openjdk-devel
$ wget https://downloads.apache.org/kafka/3.5.2/kafka_2.13-3.5.2.tgz
$ tar -xvzf ./kafka_2.13-3.5.2.tgz
$ mv ./kafka_2.13-3.5.2 /usr/local/kafka

# 서비스 등록
$ vi /etc/systemd/system/zookeeper.service
[Unit]
Description=Apache Zookeeper server
Documentation=http://zookeeper.apache.org
Requires=network.target remote-fs.target
After=network.target remote-fs.target

[Service]
Type=simple
ExecStart=/usr/bin/bash /usr/local/kafka/bin/zookeeper-server-start.sh /usr/local/kafka/config/zookeeper.properties
ExecStop=/usr/bin/bash /usr/local/kafka/bin/zookeeper-server-stop.sh
Restart=on-abnormal

[Install]
WantedBy=multi-user.target

$  vi /etc/systemd/system/kafka.service
[Unit]
Description=Apache Kafka Server
Documentation=http://kafka.apache.org/documentation.html
Requires=zookeeper.service

[Service]
Type=simple
Environment="JAVA_HOME=/usr/lib/jvm/jre-17-openjdk"
ExecStart=/usr/bin/bash /usr/local/kafka/bin/kafka-server-start.sh /usr/local/kafka/config/server.properties
ExecStop=/usr/bin/bash /usr/local/kafka/bin/kafka-server-stop.sh

[Install]
WantedBy=multi-user.target

$ systemctl daemon-reload
$ systemctl start zookeeper
$ systemctl start kafka
$ systemctl enable zookeeper
$ systemctl enable kafka

# 환경 설정
$ vi /usr/local/kafka/config/zookeeper.properties
$ vi /usr/local/kafka/config/server.properties

# 방화벽 허용
$ firewall-cmd --permanent --zone=public --add-port=9092/tcp
$ firewall-cmd --permanent --zone=public --add-port=2181/tcp
$ firewall-cmd --reload

# 토픽 생성
$ cd /usr/local/kafka/bin
$ ./kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic event-pusher

# 테스트
$ /usr/local/kafka/bin/kafka-console-producer.sh --broker-list localhost:9092 --topic event-pusher
$ /usr/local/kafka/bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic event-pusher --from-beginning
```

## 작동 방식: Kafka vs. Redis 게시/구독

Apache Kafka는 여러 애플리케이션이 서로 독립적으로 데이터를 스트리밍할 수 있게 해주는 이벤트 스트리밍 플랫폼입니다. *생산자* 및 *소비자*라고 하는 이러한 애플리케이션은 *주제*라고 하는 특정 데이터 파티션에 정보를 게시하고 구독합니다.

한편 Redis는 애플리케이션 간 지연 시간이 짧은 데이터 전송을 지원하는 인 메모리 데이터베이스로 설계되었습니다. 모든 메시지를 하드 디스크 대신 RAM에 저장하여 데이터 읽기 및 쓰기 시간을 줄입니다. Kafka와 마찬가지로 여러 소비자가 Redis 스트림을 구독하여 메시지를 검색할 수 있습니다.

게시/구독 메시징에 둘 다 사용할 수 있지만 Kafka와 Redis는 다르게 작동합니다.

## Kafka 워크플로

Apache Kafka는 컴퓨팅 클러스터를 통해 생산자와 소비자를 연결합니다. 각 클러스터는 서로 다른 서버에 있는 여러 Kafka 브로커로 구성됩니다.

Kafka는 다음과 같은 목적으로 주제 및 파티션을 생성합니다.

- 이메일, 결제, 사용자, 구매 등 관심 주제에 속하는 유사 데이터를 그룹화하는 주제
- 데이터 복제 및 내결함성을 위해 여러 브로커에 걸쳐 파티셔닝

생산자는 브로커에게 메시지를 게시합니다. 브로커는 메시지를 수신하면 데이터를 주제로 분류하고 데이터를 파티션에 저장합니다. 소비자는 관련 주제에 연결하여 해당 파티션에서 데이터를 추출합니다.


## Kafka 구성 요소

- Cluster
    - Pub/Sub 모델 패턴에서 메시지 관리를 담당합니다.
        - 임의 갯수의 노드로 구성되어 있는데 이 노드가 Broker입니다.
    - 1대의 Broker로 기본적인 기능 실행 가능하나, **3대 이상**의 Broker를 1개의 클러스터로 묶어 데이터를 안전하게 보관합니다.
- Event
    - Kafka에서 Producer 와 Consumer가 데이터를 주고받는 단위(메세지)입니다.
- Producer
    - Kafka에 메세지를 Kafka 클러스터에 전송합니다.
    - 메시지 전송 시 Batch 처리가 가능합니다.
    - key 값을 지정하여 특정 파티션으로만 전송이 가능합니다.
    - 전송 acks값을 설정하여 효율성을 높일 수 있습니다.
    - ACKS=0 -> 매우 빠르게 전송. 파티션 리더가 받았는 지 알 수 없습니다.
    - ACKS=1 -> 파티션 리더가 받았는지 확인합니다. 따른 설정을 하지 않으면 기본값으로 1을 가집니다.
    - ACKS=ALL -> 파티션 리더 뿐만 아니라 팔로워까지 메시지를 받았는 지 확인합니다.
- Consumer
    - Kafka 클러스터에서 Topic을 구독하고 이로부터 얻어낸 메세지를 받아 처리합니다.
    - 메세지를 Batch 처리할 수 있습니다.
    - 한 개의 Consumer는 여러 개의 Topic을 처리할 수 있습니다.
    - 메시지를 소비하여도 메시지를 삭제하지는 않습니다. Kafka delete policy에 의해 삭제됩니다. 한 번 저장된 메시지를 여러번 소비도 가능합니다.
    - Consumer는 Consumer 그룹에 속합니다.
- Topic
    - Kafka에 데이터를 구분하기 위해 사용하는 단위입니다.
    - 이벤트가 모이는 곳으로써, Producer는 topic에 이벤트를 게시하고, Consumer는 topic을 구독해 이벤트를 가져와 처리합니다.
    - 한 개의 토픽은 한 개 이상의 파티션으로 구성됩니다.
- Partition
    - Topic은 여러 Broker에 분산되어 저장되며, 이렇게 분산된 topic을 Partition이라고 합니다.
    - Partition의 목적은 분산 처리입니다.
    - Topic 생성 시 Partition 개수를 지정할 수 있습니다.
    - Partition이 1개라면 모든 메시지에 대해 순서가 보장되며, Partition 내부에서 각 메시지는 offset(고유 번호)로 구분됩니다.
    - 만약 여러개라면 Kafka 클러스터가 라운드 로빈 방식으로 분배해서 분산처리되기 때문에 순서 보장이 되지 않습니다.
    - Partition이 많을 수록 처리량이 좋지만 장애 복구 시간이 늘어납니다.
    - 저장된 데이터는 `ls /tmp/kafka-logs` 을 통해 확인할 수 있습니다.
    - Partition 폴더 안에 데이터는 아래와 같습니다.
        - log: 메세지와 메타데이터를 저장한 파일입니다.
        - index: 메시지의 offset을 인덱싱한 정보를 저장한 파일입니다.
        - timestamp: 메시지에 포함된 timestamp값을 기준으로 인덱싱한 정보를 저장한 파일입니다. timestamp 값은 Broker가 적재한 데이터를 삭제하거나 압축하는데 사용됩니다.
    - 1개의 Partition에는 Leader와 Follow 노드가 있습니다.
    - Partition 마다 replication을  설정해줄 수 있습니다.
    - Replication factor가 2 이상으로 설정되면 Partition은 Leader노드와 Follow 노드가 있는데 Leader 노드는 Producer와 Consumer를 통해 데이터를 write/read를 하며, Follow 노드는 데이터를 복제를 담당합니다.
        
        ** replicas에 첫번째는 Leader 
        
        ```java
        {
        	"version":1,
        	"partitions": [
        		{
        			"topic": "sample", "partition":0, "replicas": [1, 2]
        		}
        	]
        }
        ```
        
- Broker
    - 실행된 Kafka 서버입니다.
    - Producer와 Consumer는 별도의 애플리케이션으로 구성되는 반면, Broker는 Kafka 자체입니다.
    - Broker(각 서버)는 Kafka Cluster 내부에 존재합니다.
    - 서버 내부에 메시지를 저장하고 관리하는 역할을 수행합니다.
- Zoopeeper
    - 분산 메세지의 큐의 메타 정보를 관리합니다.
    - 쉘 명령어를 통해 어떤 데이터를 저장할지 설정할 수 있습니다.
- Offset
    - Consumer에서 메세지를 어디까지 읽었는지 저장하는 값입니다.
    - Consumer 그룹의 Consumer들은 각각의 파티션에 자신이 가져간 메시지의 위치 정보(offset) 을 기록하여 Consumer 장애 발생 이후 다시 실행되었을 때 Offset을 통해 읽었던 위치에서 다시 읽을 수 있습니다.
- Coordinator
    - 다수의 Broker 중 하나의 Broker가 수행하는 역할입니다.
    - Consumer 그룹의 상태를 확인하고 Partition과 Consumer을 매칭하여 분배해주며, 만약 Consumer가 Consumer 그룹에서 빠져 매칭이 끊기면 다른 Consumer와 Partition을 연결하여 연결이 끊기지 않도록 rebalance 작업을 해줍니다.
