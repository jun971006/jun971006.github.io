---
title:  "[Study] CS 스터디 - 디자인 패턴(옵저버 패턴 Part2)"

categories:
  - CS
tags:
  - Design pattern
  - Observer pattern
  -
last_modified_at: 2023-08-29T19:10:00-05:00

toc: true
toc_sticky: true
toc_label: "목차"
---

## 디자인 패턴 뿌시기

### 옵저버 패턴 - Part2

이전 게시글에서 다뤘던 예제에서 더 나아가 알림 순서를 조정할 수 있는 예제를 살펴보려한다.

> [이전글 보기](https://jun971006.github.io/cs/Observer-Pattern-part1)

<hr>

##### 1. 주제 객체

- 변경사항
  - 관찰자 객체 저장 객체 
    - List -> LinkedHashMap
  - 알림 순서 객체
    - 순서, 관찰자 객체
  - 알림 순서 객체 배열
    - update 함수 사용할 때 사용

    ```java
    import java.util.ArrayList;
    import java.util.HashMap;
    import java.util.LinkedHashMap; <- 추가!
    import java.util.List;
    import java.util.Map;

    // 주제 객체
    class WeatherStation {
        private Map<String, Observer> observersMap = new LinkedHashMap<>(); // 관찰자 객체를 저장할 문자열, 관찰자 객체 해시맵 생성

        private List<ObserverOrder> notificationOrder = new ArrayList<>(); // 알림 순서 지정할 문자열 배열 생성
        private String weather;

        public void addObserver(Observer observer) {
            observersMap.put(observer.getName(), observer);  // 필수 요소: 옵저버 등록 메서드           
        }

        public void removeObserver(Observer observer) {
            notificationOrder.removeIf(order -> order.getObserver().equals(observer));  // 삭제하려는 객체를 notificationOrder List 인스턴스에서 찾은 후 삭제
            observersMap.remove(observer.getName()); // 필수 요소: 옵저버 제거 메서드

        }

        // 알림 순서 객체 배열에 관찰자 객체 및 순서 추가
        public void specifyNotificationOrder(Observer observer, int order) {
            notificationOrder.add(new ObserverOrder(order, observer));
        }

        public void setWeather(String weather) {
            this.weather = weather;
            notifyObservers(); // 필수 요소: 상태 변화 시 옵저버들에게 알리는 메서드
        }

        private void notifyObservers() {
            // 알림 순서 객체 배열이 비어있을 때(default)
            // 주제 객체에 넣은 순서대로 관찰자 객체에게 알림이 간다.
            //   - LinkedHashMap으로 순서 보장
            if(notificationOrder.isEmpty()) {
                for(Observer observer : ObserversMap.values()) {
                    observer.update(weather);
                }
            }

            else {
                // 알림 순서 객체 배열에 들어있는 관찰자 객체의 order 값을 통해 정렬한 뒤 
                // update 함수 호출!
                notificationOrder.sort((o1, o2) -> Integer.compare(o1.getOrder(), o2.getOrder()));
                for(ObserverOrder observerOrder : notificationOrder) {
                    observerOrder.getObserver().update(weather);
                }
            }
        }
    }
    ```

<hr>

##### 2. <strong>관찰자 순서 객체</strong>(신규)

- 관찰자 객체 순서, 관찰자 객체를 담고 있는 클래스

    ```java
    class ObserverOrder {
        private int order;
        private Observer observer;

        public ObserverOrder(int order, Observer observer) {
            this.order = order;
            this.observer = observer;
        }

        public int getOrder() {
            return order;
        }

        public Observer getObserver() {
            return observer;
        }
    }
    ```

<hr>

##### 3. 관찰자 객체(인터페이스)

- 변경사항
  - name getter 추가

    ```java
    // 옵저버 인터페이스
    interface Observer {
        void update(String weather); // 필수 요소: 업데이트 메서드 정의
        String getName(); // getter : name
    }
    ```

<hr>

##### 4. 관찰자 객체(구현 클래스)
    
- 변경사항
  - name getter 구현

    ```java
    // 구체적인 옵저버 클래스
    class Person implements Observer {
        private String name;

        public Person(String name) {
            this.name = name;
        }

        @Override
        public void update(String weather) {
            System.out.println(name + " : " + weather);
        }

        @Override
        public String getName() {
            return name;
        }

    }
    ```

<hr>

##### 5. 옵저버 패턴 메인

- 주제 객체 생성
- 옵저버 객체 생성
- 옵저버 객체 등록
- 주제 객체 상태 변경
  - 관찰자 객체에 알림

    ```java
    public class ObserverPatternDemo {
        public static void main(String[] args) {
            WeatherStation weatherStation = new WeatherStation(); // 필수 요소: 주제 생성

            Person Person1 = new Person("person1"); // 필수 요소: 옵저버 생성
            Person Person2 = new Person("person2");
            Person Person3 = new Person("person3");

            weatherStation.addObserver(Person1); // 필수 요소: 옵저버 등록
            weatherStation.addObserver(Person2);
            weatherStation.addObserver(Person3);

            weatherStation.setWeather("sunny"); // 필수 요소: 상태 변경 (옵저버 알림)
            
            System.out.println("-----------------");

            weatherStation.specifyNotificationOrder(Person1, 2);
            weatherStation.specifyNotificationOrder(Person2, 3);
            weatherStation.specifyNotificationOrder(Person3, 1);

            weatherStation.setWeather("rainy"); // 필수 요소: 상태 변경 (옵저버 알림)
        }
    }
    ```
    
<hr>

##### 6. 결과

   ```java
   person1 : sunny
   person2 : sunny
   person3 : sunny
   -----------------
   person3 : rainy
   person1 : rainy
   person2 : rainy
   ```

<hr>

#### 결론
> 관찰자의 알림 순서를 저장하고 있는 observerOrder 객체와 순서를 지정하는 specifyNotificationOrder 메서드를 통해 직접 알림 순서를 지정할 수 있었다.

<hr>