# 우선순위 큐(Priority Queue)
우선순위 큐는 들어간 순서에 상관없이 데이터의 우선순위에 따라서 우선순위가 높은 데이터부터 꺼내도록 만들어진 큐이다.
![](https://media.vlpt.us/post-images/holicme7/b77edff0-1ff7-11ea-b8b0-5340b200eb0f/image.png)
위 사진처럼 내부 요소는 힙으로 구성되어 이진트리 구조로 이루어져 있다.  
내부구조가 힙으로 구성되어 있기에 시간 복잡도는 O(NLogN)이며, 응급실과 같이 우선순위를 중요시해야 하는 상황에서 주로 쓰인다.

## 예제코드
우선순위 큐는 힙을 이용하여 구현하는 것이 일반적이다.  
데이터를 삽입할 때 우선순위를 기준으로 최대힙 혹은 최소 힙을 구성한다.  
데이터를 꺼낼 때 루트 노드를 얻어낸 뒤 루트 노드를 삭제할 때는 빈 루트 노드 위치에 맨 마지막 노드를 삽입한 후 아래로 내려가면서 적절한 자리를 찾아서 옮기는 방식으로 진행된다. 

기본적으로 자바는 PriorityQueue를 객체로 제공한다.  
다음 코드는 PriorityQueue를 이용한 프로그래머스 더 맵게 문제이다.
```java
import java.util.PriorityQueue;

class Solution {
    public int solution(int[] scoville, int K) {
        int answer, cnt = 0;
        PriorityQueue<Integer> queue = new PriorityQueue<>();
        for (int j : scoville) {
            queue.add(j);
        }
        while (true) {
            if(queue.size() == 1 && queue.peek() < K) {
                answer = -1;
                break;
            }
            if(queue.peek() < K) {
                int num = queue.poll() + (queue.poll() * 2);
                queue.add(num);
            } else {
                answer = cnt;
                break;
            }
            cnt++;
        }
        return answer;
    }
}
```
만약 우선순위가 높은 순으로 하고 싶다면, PriorityQueue를 선언할 때 new PriorityQueue<>(Collections.reverseOrder())로 선언하면 된다.