# kafka

kafka 整合環境

## use

setting env

```
cp .env.example .env
```

run kafka

```
docker-compose up -d 
```

## environment

### kafka environment

相關設定可以參考KafkaConfig，並在前面加上`KAFKA_`，單詞與單詞之間使用`_`進行分隔，然後一率為大寫。

EX:

```bash
# 設定kafka broker id
KAFKA_BROKER_ID=1
# 設定kafka日誌目錄
KAFKA_LOG_DIRS: /kafka
```

下面為kafka常用設定參數

| 參數 | 說明 | Example |
| ---- | --- | --- |
| broker.id | broker id(每一個kafka都是一個broker，且在叢集中broker id是不能重複的) | broker.id=0 |
| listeners| 監聽器 設定kafka service訪問位址(host+port) | listeners=INSIDE://:9092,OUTSIDE://:9094 |
| advertised.listeners | 允許對外的 監聽器 | advertised.listeners=PLAINTEXT://localhost:9092 |
| listener.security.protocol.map| 監聽器名稱與和安全協議之间的map集合。 | INSIDE:PLAINTEXT,OUTSIDE:PLAINTEXT |
| inter.broker.listener.name | 如果自定義了監聽器名稱後，則需要使用該參數，以確保內部broker能互通 | inter.broker.listener.name=INTERNAL |
| log.dir | kafka 日誌目錄位置 | log.dirs=/usr/local/var/lib/kafka-logs |
| log.dirs | 同log.dir，可擇一設定就好 | |
| log.retention.ms | log保留時間(毫秒) | log.retention.ms=168 |
| log.retention.minutes | log保留時間(分鐘) | log.retention.minutes=168 |
| log.retention.hours | log保留時間(小時)，log預設保留時間為168小時 | log.retention.hours=168 |
| auto.create.topics.enable | 是否允許自動建立tpoic；當該值為true時，如果producer向尚未topic發送訊息時，則kafka會自動建立一個名<num.partitions>的topic。對於kafka管理上考量建議該值設為false| auto.create.topics.enable: false |
| message.max.bytes | 允許最大訊息size | message.max.bytes: 1048576|
| min.insync.replicas | Kafka Partition的最小同步副本數 | |
| acks | 生產者模式，0:只傳送不管有沒有寫入、寫入到leader就認為成功、-1/all：所有觀察者都同步過去，才算成功|  |
| zookeeper.connect | zookeeper service 連結位置(位址) | zookeeper.connect=localhost:2181 |
| zookeeper.connection.timeout.ms | zookeeper service 連線逾期時效 | zookeeper.connection.timeout.ms=18000 |
| zookeeper.session.timeout.ms |  zookeeper service session 逾期時效 | zookeeper.session.timeout.ms=30000 |

除了原本KafkaConfig所提供可以設定環境變數之為，另外kafka的docker image也令外提供了幾個參數。

| 參數 | 說明 |  |
| ---- | --- | --- |
| KAFKA_CREATE_TOPICS | 啟動kafka時一併建立topic | KAFKA_CREATE_TOPICS: Topic1:1:3,Topic2:1:1:compact |
| KAFKA_ADVERTISED_HOST_NAME | kafka host ip，也可以直接填寫kafka| KAFKA_ADVERTISED_HOST_NAME:localhost |
| KAFKA_ADVERTISED_PORT | kafka host port | KAFKA_ADVERTISED_PORT: 9092 |

### zookeeper environment

與kafka的docker image一樣，zookeeper相關設定可以參考zookeeper的設定，並且前面加上`ZOOKEEPER_`字串，然後單詞與單詞之間使用`_`進行分隔，且一率為大寫字母。

| 參數 | 說明 |  |
| ---- | --- | --- |
| tickTime | Zookeeper与客户端的心跳间隔时间（单位毫秒）。 | tickTime=2000 |
| initLimit | Zookeeper集群中Leader和Follower之间初始连接的最大心跳数量 | initLimit=10 |
| syncLimit | Zookeeper集群中Leader和Follower之间的最大心跳数量 | syncLimit=5 |
| dataDir | 資料儲存位置| dataDir=/usr/local/var/lib/zookeeper |
| clientPort | 監聽客户端連線用的port號 | clientPort=2181 |
| maxClientCnxns | 指定dataDir中保留的快照数量 | maxClientCnxns=60 |
| autopurge.purgeInterval | 清除任務時效(小時)，`0`表示停用自動清除功能| autopurge.purgeInterval=1 |
| metricsProvider.className | 設定為`org.apache.zookeeper.metrics.prometheus.PrometheusMetricsProvider`後以啟用Prometheus.io導出器。| metricsProvider.className=org.apache.zookeeper.metrics.prometheus.PrometheusMetricsProvider |
| metricsProvider.httpPort | Prometheus.io導出器會啟動启Jetty 服務並綁定至該port號 | metricsProvider.httpPort=7000 |
| metricsProvider.exportJvmInfo | 是否允許Prometheus.io導出JVA相關指標| metricsProvider.exportJvmInfo=true |

## REFERENCE

1. [KafkaConfig](https://jaceklaskowski.gitbooks.io/apache-kafka/content/kafka-server-KafkaConfig.html)

2. [kafka Documentation](https://kafka.apache.org/documentation)

3. [kafka listeners 和 advertised.listeners 的应用](https://segmentfault.com/a/1190000020715650)

4. [Kafka解析之topic创建(1)](https://blog.csdn.net/u013256816/article/details/79303825)

5. [Kafka 参数解释--min.insync.replicas](https://www.cnblogs.com/juniorMa/articles/14120886.html)

6. [Kafka 笔记 01: Replication](https://fleurer.github.io/2020/03/07/kafka01-replication/)

7. [wurstmeister/kafka](https://hub.docker.com/r/wurstmeister/kafka/)

8. [Zookeeper系列(1)：安装与介绍](https://www.cnblogs.com/seve/p/14701635.html)
