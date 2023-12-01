---
title:  "[Study] CS 스터디 - 프로그래밍 패러다임(객체 지향 프로그래밍)"

categories:
  - CS
tags:
  - Object Oriented Programming

last_modified_at: 2023-08-31T19:10:00-05:00

toc: true
toc_sticky: true
toc_label: "목차"
---


## 프로그래밍 패러다임

### 객체 지향 프로그래밍 뿌시기

객체 지향 프로그래밍이란 무엇인가

소프트웨어 개발을 한번이라도 해본 사람은 들어봤을것이다.

공부를 했더라도 직접 코드에 적용을 하려고 하면 다시 까먹고 구글에 검색하는 모습을 자주 볼 수 있었다.

이번 기회에 객체 지향 프로그래밍에 대해서 완벽하게 짚고 넘어가려고 한다!

#### 객체 지향 프로그래밍이란
객체 지향 프로그래밍에 대해 알기 전에 우선 객체란 무엇일까?

객체란 다른것과 식별 가능한 물리적으로 존재하거나 추상적으로 생각할 수 있는 것! 이라고 한다. 
<br>쉽게 말해 현실 세계에서 눈으로 보이는 사물이나 사람 모두 해당된다.
<br> 이걸 프로그래밍적으로 바꿔서 설명하면 클래스에서 정의한 것을 토대로 메모리에 할당된 것을 말한다.

말로만 설명해서 아직 감이 안오지만 

장점부터 알아보자!

#### 장점
##### 1. 유지보수에 용이
자동차가 고장났다고 상황을 가정했을 때, 정비사는 다른 부품은 건들지않고, 
<br> 해당하는 부품만 고치거나 바꿔 끼우면 자동차는 다시 정상적으로 운행이 가능해진다. 
<br><br> 여기서 부품을 클래스라고 생각하면 첫 번째 장점을 이해할 수 있을것이다.

##### 2. 코드의 재사용
자동차 회사가 2.0 디젤 엔진을 사용해서 자동차를 만드는데, 만들때마다 새로운 엔진을 개발한다면 1년에 100대도 팔지 못할것이다..
<br><br> 엔진을 재사용해서 자동차를 만드는것과 같이 프로그래밍에서는 상속이라는 개념을 사용해 기존 클래스의 코드를 재사용 할 수 있다. 
<br> 이 부분은 뒤에서 예제와 함께 추가로 이해해보자!

##### 3. 생산성 향상
자동차를 객체로 표현하면 엔진, 차체, 휠, 타이어 등과 같이 개별적인 요소로 표현할 수 있다. 
<br><br> 이런식으로 객체라는 단어 자체가 현실 세계의 개념을 기반으로 설계한 것이기 때문에 구조를 이해하기 쉽고, 
<br> 각 요소들을 조합해서 다양한 자동차(객체)를 만들 수 있다.

이러한 객체 지향 프로그래밍의 특징으로는 추상화, 상속, 다형성, 캡슐화 총 4가지가 있는데, 
<br> 이 특징들이 객체 지향 프로그래밍의 장점들을 잘 살릴 수 있는 방향으로 발전해왔다고 볼 수 있다.

#### 특징
객체 지향 프로그래밍의 특징은 예제와 함께 이해해보자

##### 1. 추상화
추상화는 복잡한 시스템에서 중요한 세부사항을 감추고 핵심 기능에만 집중하는 것을 의미합니다. 자동차의 예시에서, 운전자는 자동차의 내부 동작 메커니즘을 알 필요 없이 조작할 수 있습니다.

```Java
abstract class Car {
    String make;
    String model;

    abstract void start();
    abstract void accelerate();
    abstract void brake();
}

```

##### 2. 상속
상속은 기존 클래스의 속성과 메서드를 재사용하거나 확장하여 새로운 클래스를 만드는 개념입니다. 자동차의 예시에서, 다양한 유형의 자동차는 기본 Car 클래스를 확장할 수 있습니다.

```Java
class SUV extends Car {
    @Override
    void start() {
        System.out.println("SUV started.");
    }

    @Override
    void accelerate() {
        System.out.println("SUV is accelerating.");
    }

    @Override
    void brake() {
        System.out.println("SUV is braking.");
    }
}

class Sedan extends Car {
    @Override
    void start() {
        System.out.println("Sedan started.");
    }

    @Override
    void accelerate() {
        System.out.println("Sedan is accelerating.");
    }

    @Override
    void brake() {
        System.out.println("Sedan is braking.");
    }
}
```

##### 3. 다형성
다형성은 다른 클래스의 객체가 같은 메서드를 호출할 때 서로 다르게 동작할 수 있는 능력을 말합니다. 자동차의 예시에서, 서로 다른 자동차 유형은 같은 메서드를 호출해도 다른 방식으로 동작할 수 있습니다.

```Java
public class CarMain {
    public static void main(String[] args) {
        Car sedan = new Sedan();
        Car suv = new SUV();

        performCarActions(sedan);
        performCarActions(suv);
    }

    static void performCarActions(Car car) {
        car.start();
        car.accelerate();
        car.brake();
    }
}

```

##### 4. 캡슐화
캡슐화는 데이터와 관련 메서드를 하나의 단위로 묶어 외부로부터 보호하는 개념입니다. 자동차의 예시에서, 클래스의 내부 데이터와 동작은 외부로부터 감춰지고 필요한 기능만 노출됩니다.

```Java
class Car {
    private String make;
    private String model;

    public Car(String make, String model) {
        this.make = make;
        this.model = model;
    }

    public void start() {
        System.out.println("Car started.");
    }

    public void accelerate() {
        System.out.println("Car is accelerating.");
    }

    public void brake() {
        System.out.println("Car is braking.");
    }
}

```

#### SOLID
객체 지향 프로그래밍에 대해서 공부를 할 때 빠질 수 없는게 바로 SOLID 라는 5가지 원칙입니다.
<br> 이러한 원칙을 지켜서 개발을 하면 소프트웨어를 더 견고하고 유지보수하기 편하게 해줍니다.

##### 1. SRP
SRP, Single Responsibility Principle, 단일 책임 원칙은 클래스나 모듈을 만들 때 하나의 책임만 가지도록 설계해야 하는것을 의미합니다.

###### 예시
고객이 자동차를 주문하는 과정을 살펴보면 
1. 고객의 자동차 주문
2. 주문에 대한 검증
3. 자동차 주문 처리 등 

주문이라는 프로세스 안에 있는 상세한 기능들은 독립적인 책임만 가지도록 나눠서 개발합니다.

이렇게 개발하게 되면 추후에 수정소요에 대한 예측이 가능해지며, 유지보수가 쉬워지는 효과가 생깁니다.

하지만, 과도한 분리로 클래스 수가 증가되어 책임 분배가 어려워질 수 있습니다.

##### 2. OCP
OCP, Open/Closed Principle, 개방 폐쇄 원칙은 소프트웨어 요소(클래스, 모듈 등)는 확장 가능성은 열어두되, 수정 가능성은 닫아야 한다는 원칙입니다.

이는 기존의 클래스에 새로운 기능을 추가할 때, 기존 코드는 수정하지 않으면서 확장해야 한다는 의미입니다.

하지만 인터페이스의 설계와 추상화가 필요하기 때문에, 초기 구현이 복잡할 수 있습니다.

##### 3. LSP
LSP, Liskov Substitution Principle, 리스코프 치환 원칙은 하위 클래스가 상위 클래스의 기능을 변경하지 않도록 하는 원칙입니다.

이는 상속 관계를 유지하면서 하위 클래스가 상위 클래스의 기능을 변경하지 않도록 합니다.

##### 4. ISP
ISP, Interface Segregation Principle, 인터페이스 분리 원칙은 클라이언트는 자신이 사용하지 않는 인터페이스에 의존성을 강요받지 않도록 하는 원칙입니다.

이는 큰 기능을 작은 인터페이스별로 쪼개서 설계하여 클라이언트가 필요한 기능만 사용할 수 있게 해줍니다.

하지만, 인터페이스의 수가 늘어나 코드 관리가 복잡해질 수 있습니다.

##### 5. DIP
DIP, Dependency Inversion Principle, 의존성 역전 원칙은 고수준의 모듈(ex) 비즈니스 로직 등)이 저수준의 모듈(ex) 데이터베이스, 파일 입출력 등)에 의존해서는 안되는 원칙입니다. 

두 모듈 모두 추상화(인터페이스)에 의존해야 하며, 구체적인 구현이 아닌 인터페이스를 통해 상호작용하여, 시스템을 보다 유연하고 확장 가능하게 만들어주며, 코드의 결합도를 낮추는 역할을 합니다.

하지만, 인터페이스와 추상화의 설계가 필요하므로 초기에 추가 노력이 필요할 수 있습니다.

#### 결론

객체 지향에 대한 특징과 각 특징에 대한 장/단점을 살펴볼 수 있는 시간이었고, 결국 객체 지향이라는 것은 실제 사람이 살아가는 특성과 유사하게
<br> 프로그래밍을 접목시켜서 유지보수성, 확장성, 재사용성 등을 향상시키려는 노력이라고 생각합니다.
<br>
앞으로 개발을 할 때에는 이렇게 정리한 객체 지향이라는 개념을 최대한 활용해서 개발을 진행해야겠다고 생각합니다.