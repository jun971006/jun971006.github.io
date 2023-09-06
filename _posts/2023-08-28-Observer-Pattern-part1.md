---
title:  "[Study] CS 스터디 - 디자인 패턴(옵저버 패턴 Part1)"

categories:
  - CS
tags:
  - Design pattern
  - Observer pattern
  -
last_modified_at: 2023-08-28T19:10:00-05:00

toc: true
toc_sticky: true
toc_label: "목차"
---


## 디자인 패턴 뿌시기

### 옵저버 패턴 - Part1

#### 개념
관찰자(Observer)의 상태를 주체(Subject)의 상태 변경에 따라 최신 상태를 유지하도록 도와주는 디자인 패턴이고,<br>
객체 간에 1:N의 종속성을 설정하므로, 주체의 상태가 변하면 주체에게 종속되어있는 모든 관찰자의 상태가 자동으로 업데이트 됩니다.

<hr>

#### 장점
1. 느슨한 결합(Loose Coupling)
2. 유연성과 확장성
3. 실시간 업데이트 등

> 느슨한 결합(Loose Coupling)
   주체 객체는 관찰자 객체가 공통 인터페이스를 구현한다는 사실 외에 아무것도 알 필요가 없습니다. <br>
   다른 부분에 영향을 주지 않으면서 쉽게 변경할 수 있으므로, 유연하고 관리하기 쉽습니다.

<hr>

#### 단점
1. 예기치 않은 업데이트
2. 디버깅 복잡성 등

> 신중하게 구현하지 않으면 관찰자가 불필요한 업데이트를 지속적으로 수신해서 성능 문제가 발생할 수도 있습니다.

<hr>

#### 필수요소
1. 주체 객체
2. 관찰자 객체(인터페이스)
3. 관찰자 객체(구현체)
4. 관찰자 등록 / 등록 취소
5. 관찰자에게 알림
6. 주체 상태 관리
7. 알림 순서 등

<br>
아직 잘모르겠으니 예제를 보면서 이해해봅시다.

<hr>

#### 예제
기상청 어플을 사용하는 사람으로 가정을 해봅시다.

이 기상청 어플은 날씨가 바뀌면 어플을 설치한 사람에게 바뀐 날씨를 알림으로 알려줍니다.

이 때 기상청 어플을 주체 객체라고 볼 수 있고,
사람을 관찰자라고 생각할 수 있겠습니다.

실제 예제 코드를 보면서 이해해봅시다.

<hr>

##### 1. 주체 객체

- 옵저버 객체 관리 리스트
- 옵저버 등록 / 등록 해제 메서드
- 주체 객체 상태 변화 메서드
  - 주체 객체 상태 변화 시 알림 메서드

  ```Java
    import java.util.ArrayList;
    import java.util.List;

    // 주체 객체
    class WeatherStation {
        private List<Observer> observers = new ArrayList<>(); // 필수 요소: 옵저버 객체들을 보관하는 리스트
        private String weather;

        public void addObserver(Observer observer) {
            observers.add(observer); // 필수 요소: 옵저버 등록 메서드
        }

        public void removeObserver(Observer observer) {
            observers.remove(observer); // 필수 요소: 옵저버 제거 메서드
        }

        public void setWeather(String weather) {
            this.weather = weather;
            notifyObservers(); // 필수 요소: 상태 변화 시 옵저버들에게 알리는 메서드
        }

        private void notifyObservers() {
            for (Observer observer : observers) {
                observer.update(weather); // 필수 요소: 각각의 옵저버에게 알림
            }
        }
    }
  ```

<hr>

##### 2. 관찰자 객체(인터페이스)

- 주체 객체의 상태 변화에 따른 관찰자 객체 상태 변화 메서드
  - update 메서드에 날씨값인 weather를 받아옵니다.

  ```Java
    // 옵저버 인터페이스
    interface Observer {
        void update(String weather); // 필수 요소: 업데이트 메서드 정의
    }
  ```

<hr>

##### 3. 관찰자 객체(구현 클래스)

- 관찰자 인터페이스 구현 클래스
- 상태 변화 메서드 구현

  ```Java
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
    }
  ```

<hr>

##### 4. 옵저버 패턴 메인

- 주체 객체 생성
- 옵저버 객체 생성
- 옵저버 객체 등록
- 주체 객체 상태 변경
  - 관찰자 객체에 알림

  ```Java
    public class ObserverPatternDemo {
        public static void main(String[] args) {
            WeatherStation weatherStation = new WeatherStation(); // 필수 요소: 주체 생성

            Person Person1 = new Person("person1"); // 필수 요소: 옵저버 생성
            Person Person2 = new Person("person2");

            weatherStation.addObserver(Person1); // 필수 요소: 옵저버 등록
            weatherStation.addObserver(Person2);

            weatherStation.setWeather("sunny"); // 필수 요소: 상태 변경 (옵저버 알림)
            
            System.out.println("-----------------");
            weatherStation.setWeather("rainy"); // 필수 요소: 상태 변경 (옵저버 알림)
        }
    }
  ```

<hr>

##### 5. 결과

   ```Java
   person1 : sunny
   person2 : sunny
   -----------------
   person1 : rainy
   person2 : rainy
   ```


하지만 여기서는 관찰자 객체를 주체 객체에 추가한 순서대로만 알림을 줄 수 있기 때문에 다음 코드를 추가해서 알림 순서를 조절할 수 있습니다.
<br>
to be continue...
> [다음글 보기](https://jun971006.github.io/cs/Observer-Pattern-part2)

<hr>

#### Reference 
[TheBook - 면접을 위한 CS 전공지식 노트 - 옵저버 패턴](https://thebook.io/080326/0020/) <br/>
[chatGPT](https://chat.openai.com/)

<hr>