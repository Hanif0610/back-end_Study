# 큐(Queue)
큐는 먼저 들어온 것이 먼저 나가는 선입선출(FIFO) 방식의 자료구조 이다.  
한쪽 끝(rear)에서는 삽입만 가능하며, 다른 한쪽 끝(front)에서는 삭제만 가능하다.
![image](https://user-images.githubusercontent.com/52641909/111591088-527e6e00-880a-11eb-9982-889fe1cdceb3.png)
위 그림 처럼 Queue는 rear쪽으로 item이 삽입이 되고, front쪽에서 데이터가 빠져나가는 것을 확인할 수 있다.

큐의 종류에는 위에서 설명한 선형큐, 원형큐(Circular Queue), 우선순위 큐(Priority Queue)가 존재한다.

## 장단점
- 장점
    - 데이터의 삽입/ 삭제가 빠르다.
- 단점
    - Queue의 중간에 위치한 데이터로 접근하기가 어렵다.
    - 데이터가 다 차있으면 삽입을 할 수가 없다.

## 활용
큐는 주로 데이터가 입력된 시간 순서대로 처리해야 할 필요가 있는 상황에 이용한다.
- 프린터와 같은 우선순위 작업 예약
- 은행 업무
- 콜센터 고객 대기시간
- 프로세스 관리
- 너비 우선 탐색(BFS, Breadth-First Search) 구현
- 캐시(Cache) 구현

## 예제 코드
자바에서는 Queue를 제공을 해준다.
```java
import java.util.LinkedList;
import java.util.Queue;

public class QueueEx {
    public static void main(String[] args) {
        Queue<Integer> queue = new LinkedList<>();
        queue.add(1);
        queue.add(2);
        queue.add(4);
        queue.add(3);

        while(!queue.isEmpty()) {
            System.out.println(queue.poll());
        }
    }
}
```
만약 Queue 객체를 사용하지 않고, 구현을 한다면 다음과 같이 구현할 수 있다.
```java
public class Queue {

    private int[] array = new int[100];
    int lastIdx = 0;

    public void add(int input) {
        array[lastIdx] = input;
        lastIdx++;
    }

    public int peek() {
        return array[0];
    }

    public int poll() {
        int rs = array[0];

        for(int i=1; i<=lastIdx; i++) {
            array[i-1] = array[i];
        }
        lastIdx--;

        return rs;
    }

    public boolean isEmpty() {
        return lastIdx==0?true:false;
    }
}
```