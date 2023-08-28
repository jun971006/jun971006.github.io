---
title:  "[Study] CS 스터디 - 디자인 패턴"

categories:
  - Study
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

### 옵저버 패턴

#### 개념
관찰자(Observer)의 상태를 주제(Subject)의 상태 변경에 따라 최신 상태를 유지하도록 도와주는 디자인 패턴이고,
객체 간에 1:1 or 1:N의 종속성을 설정하므로, 주제의 상태가 변하면 주제에게 종속되어있는 모든 관찰자의 상태가 자동으로 업데이트 됩니다.

#### 장점
1. 느슨한 결합(Loose Coupling)
2. 유연성과 확장성
3. 실시간 업데이트 등

> 느슨한 결합(Loose Coupling)
   주제 객체는 관찰자 객체가 공통 인터페이스를 구현한다는 사실 외에 아무것도 알 필요가 없습니다. 
   다른 부분에 영향을 주지 않으면서 쉽게 변경할 수 있으므로, 유연하고 관리하기 쉽습니다.

#### 단점
1. 예기치 않은 업데이트
2. 디버깅 복잡성 등

> 신중하게 구현하지 않으면 관찰자가 불필요한 업데이트를 지속적으로 수신해서 성능 문제가 발생할 수도 있습니다.

#### 필수요소
1. 주제 객체
2. 관찰자 객체(인터페이스)
3. 관찰자 객체(구현체)
4. 관찰자 등록 / 등록 취소
5. 관찰자에게 알림
6. 주체 상태 관리
7. 알림 순서 등

<br>
아직 잘모르겠으니 예제를 보면서 이해해봅시다.

#### 예시
기상청 어플을 사용하는 사람으로 가정을 해봅시다.

이 기상청 어플은 날씨가 바뀌면 어플을 설치한 사람에게 바뀐 날씨를 알림으로 알려줍니다.

이 때 기상청 어플을 주제 객체라고 볼 수 있고,
사람을 관찰자라고 생각할 수 있겠습니다.

실제 예제 코드를 보면서 이해해봅시다.

1. 주제 객체

    ```Java
    import java.util.ArrayList;
    import java.util.List;

    // 주제 객체
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

2. 관찰자 객체

    ```Java
    // 옵저버 인터페이스
    interface Observer {
        void update(String weather); // 필수 요소: 업데이트 메서드 정의
    }
    ```

3. 관찰자 객체(구현체)

    ```Java
    // 구체적인 옵저버 클래스
    class Kid implements Observer {
        private String name;

        public Kid(String name) {
            this.name = name;
        }

        @Override
        public void update(String weather) {
            System.out.println(name + "은(는) 날씨가 " + weather + "일 때의 상태를 보고합니다.");
        }
    }

    ```

4. 옵저버 패턴 메인

    ```Java
    public class ObserverPatternDemo {
        public static void main(String[] args) {
            WeatherStation weatherStation = new WeatherStation(); // 필수 요소: 주제 생성

            Kid kid1 = new Kid("앨리스"); // 필수 요소: 옵저버 생성
            Kid kid2 = new Kid("밥");

            weatherStation.addObserver(kid1); // 필수 요소: 옵저버 등록
            weatherStation.addObserver(kid2);

            weatherStation.setWeather("맑음"); // 필수 요소: 상태 변경 (옵저버 알림)

            weatherStation.setWeather("비"); // 필수 요소: 상태 변경 (옵저버 알림)
        }
    }
    ```

#### 