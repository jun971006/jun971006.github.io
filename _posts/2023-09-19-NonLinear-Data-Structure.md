---
title:  "[Study] CS 스터디 - 자료구조(비선형 자료구조) - 맵, 셋, 해시 테이블"

categories:
  - CS
tags:
  - Data Structure
  - NonLinear
  - Algorithm
  - Map
  - Set
  - HashTable

last_modified_at: 2023-09-19T19:10:00-05:00

toc: true
toc_sticky: true
toc_label: "목차"
---

## 비선형 자료구조
비선형 자료구조란 선형 자료구조와 다르게 일렬로 나열하지 않고 자료 순서나 관계가 복잡한 구조를 말합니다.

일반적으로 트리, 그래프 등이 있습니다.

이번에는 맵, 셋, 해시 테이블에 대해서 정리해보려고 합니다.

### 맵(Map)
맵은 Key, Value의 조합으로 형성된 자료구조입니다.

- Java에서는 'Map' 인터페이스를 통해 구현이 가능합니다.
  - 'HashMap', 'TreeMap', 'LinekedHashMap' 등이 있습니다.

```java
import java.util.HashMap;
import java.util.Map;

class MapExample {
    public static void main(String[] args) {
        // String과 Integer의 조합으로  HashMap을 생성
        Map<String, Integer> BMWs = new HashMap<>();

        // 데이터를 마구마구 넣어줍니다.
        BMWs.put("520d", 5);
        BMWs.put("X4m50d", 4);
        BMWs.put("X4m50d", 4);
        BMWs.put("m330i", 3);
        BMWs.put("220i", 2);
        BMWs.put("220i", 2);

        // HashMap 관련 메서드들을 사용해서 출력을 해줍니다.
        String carName = "520d";
        if (BMWs.containsKey(carName)) {
            int carSeries = BMWs.get(carName);
            System.out.println(carName + "'s series is: " + carSeries);
        } else {
            System.out.println(carName + " not found.");
        }

        // entrySet 메서드를 사용하면 set 형태로 반환할 수 있습니다.
        for (Map.Entry<String, Integer> entry : BMWs.entrySet()) {
            String name = entry.getKey();
            int carSeries = entry.getValue();
            
            System.out.println(name + ": " + carSeries);
        }
    }
}

```

> 결과

```java
520d's series is: 5
X4m50d: 4
m330i: 3
520d: 5
220i: 2
```

위의 결과에서 볼 수 있듯이 총 6개의 데이터를 입력했지만,
겹치지 않는 데이터만 출력된것은 entrySet 메서드는 set형태로 반환을 하기 때문입니다.

### 셋(set)
맵에서 entrySet 메서드로 다뤘듯이 특정 순서에 따라 중복되는 요소는 없고 오로지 유니크한 값만 저장하는 자료구조입니다.

- Java에서는 HashSet, LinkedHashSet, TreeSet 등이 있습니다.

```java
import java.util.HashSet;
import java.util.Set;

class HashSetExample {
    public static void main(String[] args) {
        //HashSet 생성
        Set<String> set = new HashSet<>();

        // 데이터 추가
        set.add("Apple");
        set.add("Banana");
        set.add("Cherry");
        set.add("Date");

        // 중복되는 Banana데이터 추가
        set.add("Banana");

        // 사이즈 확인
        System.out.println("Set size : " + set.size());

        // HashSet 데이터 출력
        for (String element : set) {
            System.out.println(element);
        }

        // 데이터 삭제
        set.remove("Cherry");

        // 데이터 포함여부 확인
        boolean containsDate = set.contains("Date");
        System.out.println("Set 'Date'? " + containsDate);

        // HashSet 비우기
        set.clear();

        // 비어있는지 확인
        boolean isEmpty = set.isEmpty();
        System.out.println("Set empty? " + isEmpty);
    }
}

```

> 결과

```java
Set size : 4
Apple
Cherry
Date
Banana
Set 'Date'? true
Set empty? true 
```

### 해시테이블(HashTable)
해시테이블은 key, value 조합으로 데이터를 저장하는 자료구조입니다.

내부적으로 배열을 사용해서 데이터를 저장하기 때문에 빠른 검색속도를 제공해줍니다.

각각의 Key값에 해시함수를 적용해서 배열의 고유한 index값을 주는데,
<br>미리 계산한 index를 활용해 값을 저장하거나 검색하기 때문에 해시 함수를 1번만 사용하게 됩니다.

따라서 해시테이블의 데이터 검색 시간 복잡도는가 O(1)입니다.


```java
import java.util.Hashtable;

public class HashtableExample {
    public static void main(String[] args) {
        // Hashtable 생성
        Hashtable<String, Integer> hashtable = new Hashtable<>();

        // 키-값 쌍 추가
        hashtable.put("One", 1);
        hashtable.put("Two", 2);
        hashtable.put("Three", 3);

        // 값 가져오기
        int value = hashtable.get("Two");
        System.out.println("Value for key 'Two': " + value);

        // 키 존재 여부 확인
        boolean containsKey = hashtable.containsKey("Four");
        System.out.println("Contains key 'Four': " + containsKey);

        // 키-값 쌍 제거
        hashtable.remove("Three");

        // 모든 키-값 쌍 출력
        for (String key : hashtable.keySet()) {
            int val = hashtable.get(key);
            System.out.println("Key: " + key + ", Value: " + val);
        }

        // Hashtable 크기 확인
        int size = hashtable.size();
        System.out.println("Hashtable size: " + size);
    }
}

```

>결과

```java
Value for key 'Two': 2
Contains key 'Four': false
Key: One, Value: 1
Key: Two, Value: 2
Hashtable size: 2
```

### HashMap vs HashTable

- HashTable Put
   ```java
    // HashTable put
    public synchronized V put(K key, V value) {
        // Make sure the value is not null
        if (value == null) {
            throw new NullPointerException();
        }
        // Makes sure the key is not already in the hashtable.
        Entry<?,?> tab[] = table;
        int hash = key.hashCode();
        int index = (hash & 0x7FFFFFFF) % tab.length;
        @SuppressWarnings("unchecked")
        Entry<K,V> entry = (Entry<K,V>)tab[index];
        for(; entry != null ; entry = entry.next) {
            if ((entry.hash == hash) && entry.key.equals(key)) {
                V old = entry.value;
                entry.value = value;
                return old;
            }
        }
        addEntry(hash, key, value, index);
        return null;
    }
    ```

- HashMap put
  ```java
  // HashMap put
  public V put(K key, V value) {
      return putVal(hash(key), key, value, false, true);
  }  
  ```

- 둘의 차이는 동기화 지원 여부(synchronized)에 있는데, 
<br>병렬 처리를 하면서 자원의 동기화를 고려해야 하는 상황이라면 해시테이블(HashTable)을 사용하고, 
<br>병렬 처리를 하지 않거나 자원의 동기화를 고려하지 않는 상황이라면 해시맵(HashMap)을 사용하면 된다.



### Reference
[망나니개발자 - 해시테이블](https://mangkyu.tistory.com/102) <br>
[chatGPT](https://chat.openai.com/)