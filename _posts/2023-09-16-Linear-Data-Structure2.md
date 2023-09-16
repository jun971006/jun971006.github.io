---
title:  "[Study] CS 스터디 - 자료구조(선형 자료구조) - 배열, 벡터"

categories:
  - CS
tags:
  - Data Structure
  - Linear
  - Algorithm
  - Array
  - Vector

last_modified_at: 2023-09-16T19:10:00-05:00

toc: true
toc_sticky: true
toc_label: "목차"
---

이전 게시물에 이어서 배열과 벡터에 대해 알아보겠습니다.

> [이전글 보기](https://jun971006.github.io/cs/Linear-Data-Structure1)

## 배열
배열이란 동일한 데이터 타입의 여러 값을 저장하는 자료구조입니다. 
<br>인덱스를 통해 접근할 수 있으며, 배열은 순차적 접근과 랜덤 접근이 있습니다.

- 순차적 접근 : 말 그대로 순서대로 접근해서 찾는 형태라고 볼 수 있습니다.
- 랜덤 접근 : 배열에 순차적인 데이터가 있을 때 해당하는 인덱스에 접근할 수 있는 기능입니다. -> 시간 복잡도가 O(1)

```java
class Main {
    public static void main(String[] args) {
        int[] numbers = new int[5]; // 배열 선언
         
        for(int i = 0; i< numbers.length; i++){ // 배열 값 채우기
            numbers[i] = i;
        }
        
        for (int i = 0; i < numbers.length; i++) { // 배열 값 출력
            System.out.println("numbers[" + i + "] = " + numbers[i]);
        }
    }
}
```

> 탐색 시간 : 배열 < 연결 리스트
<br>  추가/삭제 시간 : 배열 > 연결 리스트

## 벡터
벡터란 동적으로 요소를 할당할 수 있는 동적 배열입니다.

### 벡터의 특징
- 중복 허용
- 순서가 있음
- 랜덤 접근 가능
- 탐색 시간 복잡도 : O(1)

벡터는 데이터를 추가하려고 할 때 벡터가 가득 찬 상태라면 크기를 현재의 두 배로 늘립니다. 
<br>비용을 계산해보면 3이 나오는데 1보다는 크지만 상수 시간에 가깝기 때문에 
<br>O(1)이라는 시간 복잡도를 가진다고 볼 수 있습니다.

이렇게 벡터는 크기를 동적으로 관리하고, 데이터를 추가하거나 삭제하는것이 효율적입니다.
<br>-> 프로그래머가 직접 배열의 크기를 조절할 필요가 없어집니다.

Java에서는 'java.util.Vector' 클래스가 벡터를 구현하고 있습니다.

```java
import java.util.Vector; // 벡터를 쓰기 위해 추가

class Main {
    public static void main(String[] args) {
        // 벡터 생성
        Vector<Integer> vector = new Vector<>();

        // 추가 작업
        vector.add(10);
        vector.add(20);
        vector.add(1, 30); // 인덱스 지정도 가능하다

        // 
        
        for (int i = 0; i < vector.size(); i++) {
            System.out.println("vector[" + i + "] = " + vector.get(i));
        }
    }
}
```

> output

```java
vector[0] = 10
vector[1] = 30
vector[2] = 20
```