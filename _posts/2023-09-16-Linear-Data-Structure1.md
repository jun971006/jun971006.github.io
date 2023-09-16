---
title:  "[Study] CS 스터디 - 자료구조(선형 자료구조) - 리스트"

categories:
  - CS
tags:
  - Data Structure
  - Linear
  - Algorithm

last_modified_at: 2023-09-16T19:10:00-05:00

toc: true
toc_sticky: true
toc_label: "목차"
---

## 선형 자료구조
우선 자료구조에는 선형 / 비선형 자료구조가 있습니다.
여기서 선형 자료구조란 데이터가 일렬로 나열되어 있는 자료구조를 합니다.

### 종류
선형 자료구조의 종류에는 스택, 큐, 벡터, 배열, 덱, 리스트 등이 있습니다.
이 중에서 연결 리스트, 배열, 벡터를 다뤄볼 예정입니다.

#### 연결 리스트
연결 리스트는 데이터를 감싸고 있는 노드를 포인터로 연결한 자료구조입니다. 노드는 물리적으로 떨어져 있지만, 포인터를 통해 서로 연결되어 있는것처럼 사용할 수 있는 구조를 가지고 있습니다.

이러한 연결 리스트는 단순 연결, 이중 연결, 원형 이중 연결 리스트가 있습니다.

![](https://devopedia.org/images/article/409/6269.1647166159.png)

위의 그림에서 볼 수 있듯이 단순 연결 리스트와 원형 연결 리스트는 데이터의 방향이 한쪽으로만 흐르고,
 선행 노드에서 후속 노드로의 접근은 쉽지만 그 반대의 경우는 전체 노드를 거쳐서 와야하기 때문에 적절하지 않습니다.
 
이를 해결하기 위해 이중 연결 리스트가 만들어졌습니다.

##### 이중 연결 리스트 예제 코드

```java
class Node { // 노드 클래스
    int data;
    Node next;

    public Node(int data) {
        this.data = data;
        this.next = null;
    }
}

class DoublyLinkedList { // 이중 연결 리스트 클래스
    Node head;
    Node tail;

    public void append(int data) { // 노드 추가 메서드
        Node newNode = new Node(data);
        if (head == null) {
            head = newNode;
        } else {
            tail.next = newNode;
            newNode.prev = tail;
        }
        tail = newNode;
    }

    public void delete(int data) {  // 노드 삭제 메서드
        Node current = head;
        while (current != null) {
            if (current.data == data) {
                if (current.prev != null) { // 첫 번째 노드 아닐 경우
                    current.prev.next = current.next; // 포인터 이동
                } else { // 첫 번째 노드의 경우
                    head = current.next;
                }
                if (current.next != null) { // 마지막 노드 아닐 경우
                    current.next.prev = current.prev; // 포인터 이동
                } else { // 마지막 노드의 경우
                    tail = current.prev;
                }
                return;
            }
            current = current.next;
        }
    }

    public void displayForward() { // 노드 순방향 출력 메서드
        Node current = head;
        while (current != null) {
            System.out.print(current.data + " <-> "); // 데이터 출력
            current = current.next; // 포인터 이동
        }
        System.out.println("null");
    }

    public void displayBackward() { // 노드 역방향 출력 메서드
        Node current = tail;
        while (current != null) {
            System.out.print(current.data + " <-> ");
            current = current.prev;
        }
        System.out.println("null");
    }
}

class Main {
    public static void main(String[] args) {
        DoublyLinkedList doublyLinkedList = new DoublyLinkedList();

        doublyLinkedList.append(1);
        doublyLinkedList.append(2);
        doublyLinkedList.append(3);

        System.out.println("Forward: ");
        doublyLinkedList.displayForward();

        System.out.println("Deleting node with data 2.");
        doublyLinkedList.delete(2);

        System.out.println("Forward (after deletion): ");
        doublyLinkedList.displayForward();

        System.out.println("Backward (after deletion): ");
        doublyLinkedList.displayBackward();
    }
}

```

>output

```java
Forward:1 <-> 2 <-> 3 <-> null
Deleting node with data 2.
Forward (after deletion):
1 <-> 3 <-> null
Backward:
3 <-> 1 <-> null
```

다음 글에서는 배열과 벡터에 대해서 알아보겠습니다.

> [다음글 보기](https://jun971006.github.io/cs/Linear-Data-Structure2)


#### Reference 
[TheBook - 면접을 위한 CS 전공지식 노트](https://thebook.io/) <br/>
[chatGPT](https://chat.openai.com/)<br>
[devopedia.org](https://devopedia.org//)