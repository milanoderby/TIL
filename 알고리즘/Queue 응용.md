# Queue 응용 (Scheduling)

Queue 응용문제유형은 다음과 같습니다.

### 유형 정의

#### Scheduling 문제

Scheduling 에는 여러 종류가 있지만, 일반적으로 알고리즘 문제에 나오는 Scheduling 문제는 CPU Scheduling 의 알고리즘과 관련있습니다.

Scheduling 자체는 Queue 자료구조를 이용하여 구현하기 때문에 Queue 응용 문제유형 중 하나라고 말할 수 있습니다.

오늘은 Scheduling 알고리즘 중 FCFS(First Come First Served) 와 SJF(Shortest Job First)의 구현에 대해서 알아보겠습니다.

<br>

### 문제

문제의 조건이 어떻게 주어지느냐에 따라서, 어떤 스케줄링 알고리즘 방식으로 구현해야될지 달라지기 때문에 문제를 보며 설명하겠습니다.

#### FCFS (다리를 지나는 트럭)

https://programmers.co.kr/learn/courses/30/lessons/42583

#### SJF (디스크 컨트롤러)

https://programmers.co.kr/learn/courses/30/lessons/42627

<br>

### FCFS (다리를 지나는 트럭) 문제 풀이과정

#### 1. 스케줄링 알고리즘 결정

이 문제에서는 트럭이 일차선 다리를 이미 정해진 순서대로 건너는 문제이기 때문에 FCFS 방식입니다.

#### 2. 자료구조 및 변수 정의

다리 위의 트럭을 하나의 Queue로 보고, 대기하는 트럭들을 또 다른 Queue로 봅니다. 저는 이를, 각각 `bridgeQueue` 와 `waitingQueue`로 정의하겠습니다.

매 단위시간마다 2가지 Queue의 상태가 변화하므로 `time` 이라는 변수가 필요합니다.

그리고, `bridge_length`라는 변수는 사실, 다리 위의 트럭이 건너는데에 필요한 시간을 의미합니다. (프로그래머스에서 ICPC 문제를 가져올 때의 번역이 다소 좋지 않아보입니다.)

#### 3. 문제 풀이

대기트럭 큐와 다리위 트럭 큐가 비어있지 않을 때까지 아래의 작업을 반복해야합니다. (즉, 모든 대기 중인 트럭이 다리 위를 전부 건널 때까지 작업을 수행한다는 것을 의미합니다.) 매 작업마다 시간을 나타내는 변수인 `time` 값은 1씩 증가합니다.

1. 다리위의 트럭이 건너는 것과 대기트럭이 다리에 건너기 시작하는 시점이 동시에 발생가능합니다. 이 경우, 다리위의 트럭이 먼저 다리를 건너고, 대기트럭이 다리 위에 오르는 순서가 되어야 합니다.
2. 다리 위의 트럭은 (진입시점과 현재시점 간의 시간차이) 값이 `bridge_length` 보다 크거나 같을 경우, 빠져나올 수 있습니다.
3. 대기트럭은 (다리위의 트럭들의 무게 + 대기트럭의 무게) 값이 `weight` 보다 작거나 같을 경우, 다리 위에 오를 수 있습니다.
4. 트럭이 다리 위에 오르고 내리는 과정에서 다리 위 트럭들의 무게를 나타내는 변수인 `weightOfTruckOnBridge` 값과 트럭의 다리 진입시점을 나타내는 변수인 `entryTime` 등을 잘 설정해주면 쉽게 해결가능합니다.

#### 4. 구현 소스코드

```java
import java.util.*;
class Solution {
    public int solution(int bridge_length, int weight, int[] truck_weights) {
        Queue<Truck> waitingQueue = new ArrayDeque<>();
        for (int i = 0; i < truck_weights.length; i++) {
            waitingQueue.offer(new Truck(0, truck_weights[i]));
        }

        int time = 0;
        int weightOfTruckOnBridge = 0;
        Queue<Truck> bridgeQueue = new ArrayDeque<>();
        while ( !(waitingQueue.isEmpty() && bridgeQueue.isEmpty())) {

            if (!bridgeQueue.isEmpty()) {
                Truck truckOnBridge = bridgeQueue.peek();
                if ((time - truckOnBridge.entryTime) >= bridge_length) {
                    bridgeQueue.poll();
                    weightOfTruckOnBridge -= truckOnBridge.weight;
                }
            }

            if (!waitingQueue.isEmpty()) {
                Truck waitingTruck = waitingQueue.peek();
                if ((weightOfTruckOnBridge + waitingTruck.weight) <= weight) {
                    waitingQueue.poll();

                    waitingTruck.entryTime = time;
                    bridgeQueue.offer(waitingTruck);
                    weightOfTruckOnBridge += waitingTruck.weight;
                }
            }
            time++;
        }

        return time;
    }
    
    private static class Truck {
        private int entryTime;
        private int weight;

        public Truck(int entryTime, int weight) {
            this.entryTime = entryTime;
            this.weight = weight;
        }
    }
}
```

<br>

### SJF (디스크 컨트롤러) 문제 풀이과정

#### 1. 스케줄링 알고리즘 결정

이 문제의 조건은 CPU Scheduling 에서 각 작업요청의 waiting time을 최소로 하라는 것입니다.

각 작업요청의 waiting time을 최소로 하는 Scheduling 알고리즘은 SJF 알고리즘입니다. (이 문제에서는 비선점을 기반으로 하므로 SRTF 알고리즘이 아닙니다.)

#### 2. 자료구조 및 변수 정의

모든 작업요청을 요청시간 순으로 정렬하여 저장하는 1개의 PriorityQueue와 현재 시점에서 작업수행시간이 가장 짧은 순으로 정렬하여 저장하는 또 다른 PriorityQueue가 필요합니다. 이를 각각 `requestedJob`과 `jobQueue` 로 정의하겠습니다.

매 단위시간마다 Queue의 상태가 변화하므로 `time` 이라는 변수가 필요합니다.

#### 3. 문제 풀이

`requestedJob` 큐와 `jobQueue`가 비어있지 않을 때까지 아래의 작업을 반복해야합니다. (즉, 요청된 모든 작업을 수행한다는 것을 의미합니다.) 매 작업마다 시간을 나타내는 변수인 `time` 값은 1씩 증가합니다.

1. 현재시점보다 이전에 요청된 작업들을 `jobQueue` 에 모두 인입합니다.
2. `jobQueue` 에 요청이 존재하는지 확인합니다.
   1. `jobQueue` 에 요청이 존재한다면, 현재시점 이전에 요청된 작업이 있었다는 의미이므로 그 중 수행시간이 가장 짧은 작업 하나를 꺼내어 수행합니다. 작업을 수행하였으므로 시점을 작업의 수행한 시간 이후로 설정합니다.
   2. `jobQueue` 에 요청이 존재하지 않는다면, 현재시점 이전에 요청된 작업이 없었다는 의미이므로 시간 값을 증가시켜줍니다.
3. 매 작업요청을 수행할 때 마다 작업이 요청시점부터 수행완료되는 시점까지의 시간 값을 `totalWaitingTime` 변수에 더해주면, 값을 쉽게 구할 수 있습니다.

#### 4. 구현 소스코드

```java
import java.util.*;
class Solution {
    public int solution(int[][] jobs) {
        PriorityQueue<Job> requestedJob = new PriorityQueue<>(
                Comparator.comparingInt(Job::getRequestedTime));

        PriorityQueue<Job> jobQueue = new PriorityQueue<>(
                Comparator.comparingInt(Job::getBurstTime));

        for (int[] job : jobs) {
            int requestedTime = job[0];
            int burstTime = job[1];
            requestedJob.offer(new Job(requestedTime, burstTime));
        }

        int time = 0;
        int totalWaitingTime = 0;
        while (!requestedJob.isEmpty() || !jobQueue.isEmpty()) {
            while (!requestedJob.isEmpty()) {
                Job firstRequestedJob = requestedJob.peek();
                if (firstRequestedJob.getRequestedTime() <= time) {
                    jobQueue.offer(firstRequestedJob);
                    requestedJob.poll();
                } else {
                    break;
                }
            }

            if (!jobQueue.isEmpty()) {
                Job shortestJob = jobQueue.poll();
                time += shortestJob.getBurstTime();
                totalWaitingTime += time - shortestJob.getRequestedTime();
            } else {
                time++;
            }
        }
        return totalWaitingTime / jobs.length;
    }
    
    private static class Job {
        private int requestedTime;
        private int burstTime;

        public Job(int requestedTime, int burstTime) {
            this.requestedTime = requestedTime;
            this.burstTime = burstTime;
        }

        public int getRequestedTime() {
            return requestedTime;
        }

        public int getBurstTime() {
            return burstTime;
        }
    }
}
```

