# 원형 큐(Circular Queue)
선형 큐(Queue)는, 이미 사용한 영역인 front의 앞부분에 대해서 다시 활용을 못하기 때문에 메모리를 낭비한다는 단점이 있었다.  
그리고 큐가 다 찼을 경우 데이터들을 앞쪽으로 이동시켜 사용하는 방법이 있지만 남아있는 모든 데이터를 다 이동시켜야 한다는 불편한 작업을 수행해야 하기 때문에 그리 효율적으로 동작하지 못한다.  

이런 문제를 해결하고자 나온 큐가 바로 **원형 큐**이다.
![image](https://user-images.githubusercontent.com/52641909/111613749-3090e580-8822-11eb-9c9b-b53a674614fb.png)
큐의 형태가 다음과 같이 원형으로 되어 있어 원형 큐라고 한다.  
원형 큐는 처음과 끝이 연결되어 있는 형태로, 데이터가 배열의 끝에 다다르면 다시 처음으로 돌아올 수 있어 이미 사용했던 부분도 재사용이 가능하다.

![image](https://user-images.githubusercontent.com/52641909/111614707-3b984580-8823-11eb-8d6b-6613febb9eb3.png)
위 그림에서 100, 200, 300, 400, 500을 삽입하고 100, 200을 자운 상태이다.  
그림으로 표현하였을 때 오른쪽 그림이 나오게 된다.  
여기서 선형 큐였다면 더 넣을 수 있는 공간이 없었겠지만, 원형 큐이기에 빈 공간에 다시 데이터를 삽입할 수 있다.

## 장단점
- 장점
    - 선형큐와 비교하였을 때, 메모리 낭비가 발생하지 않는다.
- 단점
    - 똑같은 배열이기에 크기가 정해져있다.

## 예제코드
```java
public class CircularQueue {
    
    // 큐 배열은 front와 rear 그리고 maxSize를 가진다.
    private int front;
    private int rear;
    private int maxSize;
    private Object[] queueArray;
    
    // 큐 배열 생성
    public CircularQueue(int maxSize){
        
        this.front = 0;
        this.rear = -1;
        
        // 실제 크기보다 하나 크게 지정한다 (공백과 포화를 막기 위함)
        this.maxSize = maxSize+1;    
        this.queueArray = new Object[this.maxSize];
    }
    
    // 큐가 비어있는지 확인
    public boolean empty(){
        return (front == rear+1) || (front+maxSize-1 == rear);
    }
    
    // 큐가 꽉 찼는지 확인
    public boolean full(){
        return (rear == maxSize-1) || (front+maxSize-2 == rear);
    }
    
    // 큐 rear에 데이터 등록
    public void insert(Object item){
        
        if(full()) throw new ArrayIndexOutOfBoundsException();
        
        // rear 가 배열의 마지막이면 rear 포인터를 앞으로 돌린다.
        if(rear == maxSize-1){
            rear = -1;
        }
        queueArray[++rear] = item;
    }
    
    // 큐에서 front 데이터 조회
    public Object peek(){
        
        if(empty()) throw new ArrayIndexOutOfBoundsException();
        
        return queueArray[front];
    }
    
    // 큐에서 front 데이터 제거
    public Object remove(){
        
        Object item = peek();
        front++;
        
        // front의 다음 index가 배열크기+1 이면 처음으로 돌아간다
        if(front==maxSize){
            front = 0;
        }
        return item;
    }

}
```