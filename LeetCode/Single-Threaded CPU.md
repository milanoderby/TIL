# Single-Threaded CPU

### 개념

싱글스레드 CPU가 Task를 주어진 규칙에 맞게 처리할 때, 최종적으로 처리하는 Task 의 순서를 출력하는 문제입니다.

<br>

### 문제풀이

1. 각각의 Task는 index, 진입시간, 수행시간이라는 값을 가지고 있습니다.
2. 싱글스레드 CPU가 각각의 Task를 처리하는 규칙은 다음과 같습니다.
   - 현재 시간에 처리할 수 있는 Task가 없으면, CPU는 쉽니다.
   - CPU가 쉬고 있고, 처리할 수 있는 Task가 여러개 있다면, CPU는 그 중에서 가장 처리시간이 짧은 Task를 먼저 처리합니다. 만약, 처리시간이 같은 Task가 여러개 있다면, 그 중 index가 가장 짧은 것을 선택하여 처리합니다.
   - Task처리가 시작되면, CPU는 해당 Task를 쉬지 않고, 계속 처리합니다.
   - CPU가 어떤 Task를 끝내고, 바로 다음 Task를 처리할 수 있습니다.
3. 모든 Task를 WaitingQueue에 먼저 넣습니다. 이 때, WaitingQueue에 넣는 Task의 순서는 진입시간 순입니다.
4. WaitingQueue 에 들어있는 Task를 하나씩 꺼내어 진입시간과 현재 시간 값을 비교합니다. 진입시간이 현재시간보다 작거나 같다면, JobQueue로 이동시킵니다.
5. 만약, JobQueue가 비어있다면, 현재 시점에서 처리할 수 있는 Task가 존재하지 않다는 뜻입니다. 따라서, 현재시간을 WaitingQueue 에 들어있는 첫 번째 Task의 진입시간으로 이동시켜줍니다.
6. JobQueue가 비어있지 않다면, 현재 시점에서 처리할 수 있는 Task가 존재한다는 뜻입니다. 따라서, JobQueue 에 있는 Task 중 처리시간이 가장 짧은 것을 꺼내어 처리합니다.
7. 현재 시간은 처리한 Task의 시간만큼 늘려주고, CompletedTaskList에 완료한 Task를 추가합니다.
8. 결론적으로, CompletedTaskList의 Index를 순서대로 배열로 만들어 반환하면 답이 됩니다.

<br>

### 구현

```java
class Solution {
    public int[] getOrder(int[][] tasks) {
        List<Task> taskList = new ArrayList<>();
        for (int i = 0; i < tasks.length; i++) {
            taskList.add(new Task(i, tasks[i][0], tasks[i][1]));
        }
        taskList.sort(Comparator.comparingInt(Task::getEnqueueTime));

        Queue<Task> waitingQueue = new ArrayDeque<>(taskList);
        PriorityQueue<Task> jobQueue = new PriorityQueue<>(
            Comparator.comparingInt(Task::getProcessingTime).thenComparingInt(Task::getIndex));
        List<Task> completedTaskList = new ArrayList<>();
        int currentTime = 0;
        while (completedTaskList.size() < tasks.length) {
            // waitingQueue에 있는 Task 중 enqueueTime이 currentTime 보다 지난 것을 jobQueue로 이동합니다.
            while (!waitingQueue.isEmpty()) {
                Task waitingTask = waitingQueue.peek();
                if (waitingTask.getEnqueueTime() <= currentTime) {
                    waitingQueue.poll();
                    jobQueue.add(waitingTask);
                } else {
                    break;
                }
            }

            // jobQueue에서 꺼내 수행할 Task가 없다면, 시간을 waitingQueue에 있는 첫번째 Task로 바꿉니다.
            if (jobQueue.isEmpty()) {
                currentTime = waitingQueue.peek().getEnqueueTime();
                continue;
            }

            // jobQueue에서 꺼내 수행할 Task가 있다면, 꺼내어 수행합니다.
            Task processingTask = jobQueue.poll();
            currentTime += processingTask.getProcessingTime();
            completedTaskList.add(processingTask);
        }

        int[] result = completedTaskList.stream().mapToInt(Task::getIndex).toArray();
        return result;
    }
    
    private static class Task {

        private int index;
        private int enqueueTime;
        private int processingTime;

        public Task(int index, int enqueueTime, int processingTime) {
            this.index = index;
            this.enqueueTime = enqueueTime;
            this.processingTime = processingTime;
        }

        public int getIndex() {
            return index;
        }

        public int getEnqueueTime() {
            return enqueueTime;
        }

        public int getProcessingTime() {
            return processingTime;
        }
    }
}
```

