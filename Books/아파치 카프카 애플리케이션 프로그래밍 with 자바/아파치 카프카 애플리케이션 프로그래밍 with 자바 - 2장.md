# 아파치 카프카 애플리케이션 프로그래밍 with 자바 - 2장

## 실습환경 구축

### 1. 카프카 브로커용 서버 생성

교재에서는 AWS 인스턴스를 이용하여 카프카 브로커용 서버를 생성하였습니다.

저는 NCP(네이버 클라우드 플랫폼)을 이용하여 Linux 서버를 생성하였습니다.하였습니다.

NCP Linux 서버 생성 가이드 문서: https://www.ncloud.com/guideCenter/guide/1

어떤 방식이든 카프카 브로커로 사용할 서버를 생성하면 됩니다.

<br>

### 2. 카프카 브로커용 서버에 Java 설치

교재와 동일하게 OpenJdk 1.8버전으로 설치합니다.

```shell
$ yum install java-1.8.0-openjdk-devel.x86_64
```

<br>

### 3. 카프카 브로커용 서버에 카프카, 주키퍼 설치

교재와 동일하게 Scala 2.12버전으로 컴파일된 Kafka 바이너리 패키지 2.5.0 버전으로 다운로드합니다.

```shell
$ wget https://archive.apache.org/dist/kafka/2.5.0/kafka_2.12-2.5.0.tgz
```

<br>

다운로드된 kafka 바이너리 패키지를 압축 해제합니다.

```shell
$ tar -xvf kafka_2.12-2.5.0.tgz
```

<br>

### 4. 카프카 브로커 힙 메모리 설정

AWS Freetier 인스턴스를 이용하여 실습하고 있다면 메모리가 1GB 만 주어지기 때문에 카프카 힙 메모리 기본 설정 값인 1GB를 사용할 수 없습니다.

따라서, 아래와 같이 `KAFKA_HEAP_OPTS` 설정 값을 최소 400MB, 최대 400MB로 설정해야합니다.

```shell
$ export KAFKA_HEAP_OPTS="-Xmx400m -Xms400m"
```

<br>

위 설정을 이용하면 터미널 세션이 종료된 후, 재접속하면 `KAFKA_HEAP_OPTS` 값은 초기화되어 사라집니다.

`~/.bashrc` 파일을 이용하면 bash 쉘이 실행될 때마다 반복적으로 구동되는 파일입니다. 즉, 터미널 재접속 시에 `~/.bashrc` 파일에 명시해놓은 명령어가 실행되므로 `KAFKA_HEAP_OPTS` 변수 값 설정이 유지되는 것입니다.

```shell
# .bashrc

# User specific aliases and functions

alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'

export KAFKA_HEAP_OPTS="-Xmx400m -Xms400m"

# Source global definitions
if [ -f /etc/bashrc ]; then
        . /etc/bashrc
fi
```

<br>

위와 같이 `~/.bashrc` 파일을 편집하고 바로 적용하려면 아래 명령어를 수행합니다.

```shell
$ source ~/.bashrc
```

<br>

위와 같이 `KAFKA_HEAP_OPTS` 변수 값을 설정하지 않으면 `kafka_2.12-2.5.0/bin/kafka-server-start.sh` 파일을 통해 실행 시, 아래와 같은 구문을 통해 힙 메모리로 1GB가 할당됩니다.

```shell
if [ "x$KAFKA_HEAP_OPTS" = "x" ]; then
    export KAFKA_HEAP_OPTS="-Xmx1G -Xms1G"
fi
```

<br>

### 5. 카프카 브로커 실행 옵션 설정

실습용 카프카 브로커를 실행할 것이므로 다른 실행 옵션은 변경하지 않습니다.

다만, 카프카 클라이언트에서 브로커와 연결할 때, 사용해야하는 IP와 포트번호를 명시하기 위해 `kafka_2.12-2.5.0/config/server.properties` 파일에서 `advertised.listener` 설정 값만 아래와 같이 수정합니다.

<br>

```properties
############################# Socket Server Settings #############################

# The address the socket server listens on. It will get the value returned from
# java.net.InetAddress.getCanonicalHostName() if not configured.
#   FORMAT:
#     listeners = listener_name://host_name:port
#   EXAMPLE:
#     listeners = PLAINTEXT://your.host.name:9092
#listeners=PLAINTEXT://:9092

# Hostname and port the broker will advertise to producers and consumers. If not set,
# it uses the value for "listeners" if configured.  Otherwise, it will use the value
# returned from java.net.InetAddress.getCanonicalHostName().
advertised.listeners=PLAINTEXT://[카프카 브로커 서버 공인IP]:9092
```

<br>

### 6. 주키퍼 실행

카프카를 실행하기 전에 주키퍼를 먼저 실행하여야 합니다.

주키퍼는 카프카의 클러스터 설정 리더 정보, 컨트롤러 정보를 담고있어 카프카에 해당 정보를 제공하기 때문입니다.

일반적으로 운영환경에서는 3대를 운영해야하지만, 실습 목적이므로 현재 인스턴스에서 1대만 실행합니다.

주키퍼 실행 명령어는 아래와 같습니다. `-daemon` 옵션을 주면 백그라운드에서 실행됩니다.

```shell
$ bin/zookeeper-server-start.sh -daemon ./config/zookeeper.properties
```

<br>

### 7. 카프카 실행

```shell
$ bin/kafka-server-start.sh -daemon ./config/server.properties
```

<br>

### 8. local PC 에서 카프카와 통신 테스트

Windows OS PC인 경우 wsl을 이용하고, Mac OS PC인 경우 zsh을 이용합니다.

local PC에서 카프카 브로커 서버와 통신을 하기 위해서는 kafka 바이너리 패키지에 담겨 있는 프로그램을 이용합니다.

이를 위해 local PC에서도 Java 설치 및 kafka 바이너리 패키지를 다운로드 및 압축해제합니다.

```shell
$ yum install java-1.8.0-openjdk-devel.x86_64
```

<br>

```shell
$ wget https://archive.apache.org/dist/kafka/2.5.0/kafka_2.12-2.5.0.tgz
```

<br>

```shell
$ tar -xvf kafka_2.12-2.5.0.tgz
```

<br>

kafka 바이너리 패키지의 `kafka-broker-api-versions.sh` 명령어를 이용하여 카프카 브로커에 대한 정보를 받아오는 명령어를 수행해봅니다.

```shell
$ bin/kafka-broker-api-versions.sh --bootstrap-server 카프카 브로커 서버 공인IP:9092
```

<br>

아래와 같은 오류가 발생한다면 카프카 브로커 서버 방화벽에서 9092포트를 차단해서 발생한 오류일 수 있습니다.

```
Exception in thread "main" java.lang.RuntimeException: Request METADATA failed on brokers List
```

<br>

## 카프카 커맨드 라인 툴

### 1. 토픽 관련 명령 - `kafka-topics.sh` , `kafka-configs.sh`이용

#### 토픽 생성

```shell
bin/kafka-topics.sh \
--bootstrap-server 카프카 브로커 서버 공인IP:9092 \
--create \
--topic hello.kafka.2 \
--partitions 3 \
--replication-factor 1 \
--config retention.ms=172800000
```

<br>

1. `--create` : 토픽 생성하는 명령어임을 명시합니다.
2. `--bootstrap-server` : 통신할 카프카 클러스터 (브로커들의 모음) 서버들을 명시합니다.
3. `--topic` : 생성할 토픽의 이름을 명시합니다. (`.`과 `_` 는 동일한 문자로 인식하기 때문에 `.` 과 `_`을 혼용해서 쓰지 않도록 주의합니다.)
4. `--partitions` : 파티션의 개수를 명시합니다. 이 옵션을 명시하지 않으면 카프카 브로커 설정파일(`config/server.properties`)에 명시된 `num.partitions` 옵션의 값을 사용하여 파티션을 생성합니다.
5. `--replication-factor` : 파티션의 복제 개수를 명시합니다. 1으로 설정하면 복제를 하지 않는 것입니다. 2로 설정하면 통신하는 브로커들 중 2개의 브로커에 저장됩니다. 복제를 사용할 경우, 한 개의 브로커에 장애가 발생해도 다른 브로커에 저장된 데이터를 사용하여 안전하게 데이터 처리를 할 수 있습니다. 최대로 설정하는 복제 개수는 통신하는 `카프카 브로커의 수`입니다. 이 옵션을 명시하지 않으면 카프카 브로커 설정파일(`config/server.properties`)에 명시된 `default.replication.factor` 옵션의 값을 사용하여 파티션을 생성합니다.
6. `--config` : `kafka-topics.sh` 명령에 포함되지 않은 추가적인 설정을 할 수 있습니다. 여기서 설정한 `retention.ms` 는 토픽의 데이터를 유지하는 기간(ms 단위)을 의미합니다. 172800000ms는 2일을 의미하므로 2일이 지난 토픽의 데이터는 삭제됩니다.

<br>

#### 토픽 목록 조회

```shell
bin/kafka-topics.sh \
--bootstrap-server 카프카 브로커 서버 공인IP:9092 \
--list
```

<br>

#### 토픽 상세 조회

```shell
bin/kafka-topics.sh \
--bootstrap-server 카프카 브로커 서버 공인IP:9092 \
--topic hello.kafka.2 \
--describe
```

<br>

토픽 상세 조회 결과는 아래와 같습니다.

```
Topic: hello.kafka.2    PartitionCount: 3       ReplicationFactor: 1    Configs: segment.bytes=1073741824,retention.ms=172800000
        Topic: hello.kafka.2    Partition: 0    Leader: 0       Replicas: 0     Isr: 0
        Topic: hello.kafka.2    Partition: 1    Leader: 0       Replicas: 0     Isr: 0
        Topic: hello.kafka.2    Partition: 2    Leader: 0       Replicas: 0     Isr: 0
```

- `Partition` : 파티션의 번호를 의미합니다.
- `Leader` : 리더 파티션이 위치하는 브로커 서버의 번호를 의미합니다.
- `Replicas` : 복제된 파티션이 위치하는 브로커 서버의 번호를 의미합니다.

만약, 리더 파티션이 특정 브로커에 몰려있는 경우, 카프카 클러스터 부하가 특정 브로커들로 몰리게 됩니다. 이렇게 데이터 통신 쏠림 현상이 발생할 경우, 브로커의 네트워크 대역 이슈가 발생할 수 있습니다. **카프카 클러스터 성능이 좋지 않다면 토픽의 리더 파티션 쏠림 현상도 하나의 원인으로 의심해봐야 합니다.**

<br>

#### 토픽 파티션 개수 변경

```shell
bin/kafka-topics.sh \
--bootstrap-server 카프카 브로커 서버 공인IP:9092 \
--topic hello.kafka \
--alter \
--partitions 4
```

<br>

변경 결과 확인 명령어

```shell
bin/kafka-topics.sh \
--bootstrap-server 카프카 브로커 서버 공인IP:9092 \
--topic hello.kafka \
--describe
```

<br>

#### 토픽 리텐션 기간  변경

여기서, 중요한 점은 토픽의 옵션 중 `dynamic topic config` 라고 정의되는 일부 옵션들은 (log.segment.bytes, log.retention.ms 등)은 `kafka-topics.sh` 이 아닌 `kafka-configs.sh` 를 통해 수정할 수 있다는 점입니다.

```shell
bin/kafka-configs.sh \
--bootstrap-server 카프카 브로커 서버 공인IP:9092 \
--entity-type topics \
--entity-name hello.kafka \
--alter --add-config retention.ms=86400000
```

<br>

변경 결과 확인 명령어

```shell
bin/kafka-configs.sh \
--bootstrap-server 카프카 브로커 서버 공인IP:9092 \
--entity-type topics \
--entity-name hello.kafka \
--describe
```

<br>

### 2. 데이터 프로듀서 관련 명령 - `kafka-console-producer.sh` 이용

#### 메시지 key가 없고, 메시지 value만 produce

```shell
bin/kafka-console-producer.sh \
--bootstrap-server 카프카 브로커 서버 공인IP:9092 \
--topic hello.kafka
```

위 명령어를 수행하고 나서 메세지의 value를 입력하고 Enter 키를 누르면 하나씩 전송됩니다.

<br>

#### 메시지 key와 value 함께 produce

```shell
bin/kafka-console-producer.sh \
--bootstrap-server 카프카 브로커 서버 공인IP:9092 \
--topic hello.kafka \
--property "parse.key=true" \
--property "key.separator=:"
```

위 명령어를 수행하고 나서 key와 value를 `:` 문자로 구분하여 입력하고 Enter 키를 누르면 하나씩 전송됩니다.

관련 옵션 설정은 아래와 같습니다.

- `parse.key` : 이 값을 true로 설정하면 레코드를 전송할 때 메시지 키를 추가하여 전송할 수 있습니다.
- `key.separator` : 메시지 key와 value를 구분하는 문자를 명시합니다. 이 옵션을 설정하지 않으면 기본 설정 값은 Tab delimeter(\t)입니다.

<br>

메시지 key가 존재할 경우, key의 해시값을 이용하여 레코드를 전송할 파티션을 결정합니다. (프로듀서에서 설정된 기본 파티셔너가 이 기능을 수행합니다.)

**즉, 메시지 key가 같은 2개의 레코드를 전송할 경우, 동일한 파티션으로 전송되게 되는 것입니다.** 

**중요한 것은 파티션의 개수가 증가될 때, 위와 같은 일관성은 깨지게 됩니다.** 즉, 파티션 개수가 증가하기 전에는 `key:test`의 레코드가 1번 파티션으로 전송되었다면 파티션 개수가 증가한 후에는 `key:test`의 레코드가 2번 파티션으로 전송될 수도 있는 것입니다. 

메시지의 key를 설정할 경우 위와 같은 문제를 인식하고 주의깊게 사용하여야 됩니다.

<br>

### 3. 데이터 컨슈머 관련 명령 - `kafka-console-consumer.sh` 이용

#### 메시지 key가 없고, 메시지 value만 consume

```shell
bin/kafka-console-consumer.sh \
--bootstrap-server 카프카 브로커 서버 공인IP:9092 \
--topic hello.kafka \
--from-beginning
```

<br>

#### 메시지 key와 value 함께 consume

```shell
bin/kafka-console-consumer.sh \
--bootstrap-server 카프카 브로커 서버 공인IP:9092 \
--topic hello.kafka \
--property print.key=true \
--property key.separator="-" \
--group hello-group \
--from-beginning
```

관련 옵션 설정은 아래와 같습니다.

- `--property print.key` : 이 값을 true로 설정하면 메시지 키도 함께 확인할 수 있습니다.
- `--property key.separator` : 메시지 key와 value를 구분하는 문자를 명시합니다. 이 옵션을 설정하지 않으면 기본 설정 값은 Tab delimeter(\t)입니다.
- `--group` : 새로운 컨슈머 그룹을 생성합니다. 이 컨슈머 그룹을 통해 가져간 토픽의 메시지는 가져간 메시지에 대해 커밋을 합니다. 커밋이란 컨슈머가 특정 레코드까지 처리를 완료했다고 레코드의 오프셋 번호를 카프카 브로커에 저장하는 것입니다. 커밋 정보는 __consumer_offsets 이름의 내부 토픽에 저장됩니다.

<br>

### 4. 컨슈머 그룹 관련 명령 - `kafka-consumer-groups.sh` 이용

#### 컨슈머 그룹 목록 조회

```shell
bin/kafka-consumer-groups.sh \
--bootstrap-server 카프카 브로커 서버 공인IP:9092 \
--list
```

<br>

#### 컨슈머 그룹 상세 조회

```shell
bin/kafka-consumer-groups.sh \
--bootstrap-server 카프카 브로커 서버 공인IP:9092 \
--group hello-group \
--describe
```

<br>

컨슈머 그룹 상세 조회 결과는 아래와 같습니다.

```
GROUP           TOPIC           PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG             CONSUMER-ID     HOST            CLIENT-ID
hello-group     hello.kafka     3          3               4               1               -               -               -
hello-group     hello.kafka     2          2               4               2               -               -               -
hello-group     hello.kafka     1          1               1               0               -               -               -
hello-group     hello.kafka     0          3               4               1               -               -               -
```

- 컨슈머 그룹이 마지막으로 커밋한 토픽과 파티션이 출력됩니다. 예를 들어 첫 번째 줄은 hello.kafka 토픽의 3번 파티션에 커밋한 기록이 나옵니다.
- `CURRENT-OFFSET` : 현재 컨슈머 그룹이 커밋한 가장 마지막(최신) 레코드의 오프셋을 의미합니다. (컨슈머 그룹에서 관리하는 오프셋 번호)
- `LOG-END-OFFSET` : 현재 파티션에 들어온 가장 마지막(최신) 레코드의 오프셋을 의미합니다. (브로커에서 관리하는 오프셋 번호)
- `LAG` : `LOG-END-OFFSET` 에서 `CURRENT-OFFSET` 을 뺀 값

<br>

### 데이터 삭제 관련 명령 - `kafka-delete-records.sh` 이용

#### 삭제할 토픽, 파티션, 레코드 범위를 명시하는 파일 생성

```shell
vi delete-topic.json
```

<br>

```json
{
    "partitions": [
        {
            "topic": "hello.kafka",
            "partition": 0,
            "offset": 1
        }
    ],
    "version": 1
}
```

위와 같이 명시할 경우, `hello.kafka` 의 0번 파티션에 적재된 레코드 중 오래된 레코드부터 오프셋 1번 레코드까지 삭제하는 것을 의미합니다.

<br>

#### 토픽의 데이터 삭제

```shell
bin/kafka-delete-records.sh \
--bootstrap-server 카프카 브로커 서버 공인IP:9092 \
--offset-json-file delete-topic.json
```

- 파티션에 적재된 토픽의 데이터 중 가장 오래된 데이터(가장 낮은 숫자의 오프셋 레코드)부터 특정 시점의 오프셋 레코드까지 삭제할 수 있습니다.
- 다만, 특정 데이터(특정 오프셋 레코드)만 지정하여 삭제할 수는 없습니다.