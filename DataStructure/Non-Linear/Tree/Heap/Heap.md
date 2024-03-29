# 힙
힙 자료구조는 완전 이진 트리를 기초로 하는 자료구조이다.  
완전 이진트리는 마지막을 제외한 모든 노드에서 자식들이 꽉 채워진 이진트리를 말한다.  
여러 개 값들 중에서 최댓값 혹은 최솟값을 빠르게 찾아내도록 만들어진 자료구조이다.  
![](https://gmlwjd9405.github.io/images/data-structure-heap/types-of-heap.png)

힙은 최대 힙과 최소 힙 이렇게 2가지로 나뉘게 된다.  
- 최대 힙(max heap)
    - 부모 노드의 키 값이 자식 노드의 키 값보다 크거나 같은 완전 이진 트리이다.
    - 부모 노드의 키 값이 자식 노드의 키 값보다 크거나 같은 완전 이진 트리
- 최소 힙(min heap)
    - 부모 노드의 키 값이 자식 노드의 키 값보다 작거나 같은 완전 이진 트리
    - 부모 노드의 키 값이 자식 노드의 키 값보다 작거나 같은 완전 이진 트리

## 특징
힙에서 부모 노드와 자식 노드는 특별한 규칙이 존재한다.  
- 왼쪽 자식의 인덱스는 부모의 인덱스 * 2
- 오른쪽 자식의 인덱스는 부모의 인덱스 * 2 + 1
- 부모의 인덱스는 자식의 인덱스 / 2
![](https://gmlwjd9405.github.io/images/data-structure-heap/heap-index-parent-child.png)

힙을 구현할 때 대표적으로 사용하는 것은 배열이다.  
배열로 구현할 때, 구현을 쉽게 하기 위하여 배열의 첫 번째 인덱스인 0은 사용되지 않는다.  
만일 루트 노드의 인덱스가 0일 경우, 인덱스 2를 /2 한다 해도 0이 나올 수 없다.  
그래서 루트 노드의 인덱스의 경우는 1로 시작을 한다.

또한 특정 위치의 노드 번호는 새로운 노드가 추가되어도 변하지 않는다.  
예를 들어 루트 노드 오른쪽에 데이터가 삽입 될 경우, 그 위치의 인덱스는 항상 3이다.

## 힙 트리 삽입
힙에 새로운 요소가 들어오면, 일단 새로운 노드를 힙의 마지막 노드에 이어서 삽입한다.  
새로운 노드를 부모 노드들과 교환해서 힙의 성질을 만족시킨다.
![](https://gmlwjd9405.github.io/images/data-structure-heap/maxheap-insertion.png)

## 힙 트리 삭제
최대 힙에서 최댓값은 루트 노드이므로 루트 노드가 삭제된다.  
최대 힙(max heap)에서 삭제 연산은 최댓값을 가진 요소를 삭제하는 것이다.  
삭제된 루트 노드에는 힙의 마지막 노드를 가져온다.
힙을 재구성한다.
![](https://gmlwjd9405.github.io/images/data-structure-heap/maxheap-delete.png)