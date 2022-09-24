# 디자인 패턴
프로그램을 설계할 때 발생했던 문제들을 객체 간의 상호 관계 등을 아용하여 해결할 수 있도록 하나의 **'규약'** 형태로 만들어 놓은 것을 의미한다. 디자인 패턴을 알고 있다면 (1) 어떠한 문제를 해결할 수도 있는 방안 하나를 아는 것이고, (2) 팀원들과 효율적으로 의사소통할 수 있다.

## 싱글톤 패턴 (Singleton pattern)
하나의 클래스에 오직 **'하나'** 의 인스턴스만 가지는 패턴으로, 주로 데이터베이스 연결 모듈에 많이 사용한다.
``` typescript
class Singleton {
  private static instance: Singleton;

  private constructor() {}

  public static getInstance(): Singleton {
    if (!Singleton.instance) {
      Singleton.instance = new Singleton();
    }
    return Singleton.instance;
  }
}

const s1 = Singleton.getInstance();
const s2 = Singleton.getInstance();
console.log(s1 === s2);   // true
```

### 장점
1. 하나의 인스턴스를 공유하며 사용하기 때문에 인스턴스를 생성할 때 드는 비용(메모리, 속도 등)이 줄어든다.

### 단점
1. 의존성이 높아지며, 객제 지향 설계 원칙 중에 `개방-폐쇄 원칙`에 위배된다.
2. 단위 테스트가 어려워진다.


<br>

## 팩토리 패턴 (Factory pattern)
객체 생성 부분을 떼어낸 추상화한 패턴이다. 상속 관계에 있는 두 클래스에서 상위 클래스가 중요한 뼈대를 결정하고, 하위 클래스에서 객체 생성에 관한 구체적인 내용을 결정한다.
``` typescript
enum CaffeeType {
    Espresso,
    Latte,
}

// coffee menu
class Espresso {
    name: string = 'Espresso';
}
class Latte {
    name: string = 'Latte';
}

// coffee menu factory
class EspressoFactory {
    static createCoffee() {
        return new Espresso();
    }
}
class LatteFactory {
    static createCoffee() {
        return new Latte();
    }
}


// factory map
const CaffeeFactoryMap = {
    [CaffeeType.Espresso]: {
        factory: EspressoFactory
    },
    [CaffeeType.Latte]: {
        factory: LatteFactory
    }
}

class CoffeeFactory {
    static createCoffee(type: CaffeeType) {
        const factory =  CaffeeFactoryMap[type].factory;
        return factory.createCoffee();
    }
}

const main = () => {
    const coffee = CoffeeFactory.createCoffee(CaffeeType.Latte);
    console.log(`주문하신 ${coffee.name} 나왔습니다.`);
}

main();
```

### 장점
1. 상위 클래스와 하위 클래스가 분리되기 때문에 느슨한 결합을 가지게 된다.
2. `단일 책임 원칙`과 `개방-폐쇄 원칙`에 부합하며, 유지 보수성이 증가한다.

### 단점
1. 하위 클래스를 많이 만들어야 하기 때문에 코드가 더 복잡해질 수도 있다.


<br>

## 전략 패턴 (Stractegy pattern)
객체의 행위를 바꾸고 싶은 경우 **'직접'** 수정하지 않고 전략이라고 부르는 **'캡슐화된 알고리즘'** 을 컨텍스트 안에서 바꿔주면서 상호 교체가 가능하게 만드는 패턴이다.  
지도 길찾기를 할 때 자전거, 버스, 택시 등 대중교통마다 전부 다 다른 알고리즘과 전략을 사용하는데 전략 패턴과 비슷하다고 할 수 있다.
``` typescript
class Context {
    private strategy: ExamReport;

    constructor(strategy: ExamReport) {
        this.strategy = strategy;
    }

    public setStrategy(strategy: ExamReport) {
        this.strategy = strategy;
    }

    public doSomeBusinessLogic(): void {
        console.log('Sort...');
        const result = this.strategy.doAlgorithm([
            {
                subject: '국어',
                score: 85
            },
            {
                subject: '수학',
                score: 96
            },
            {
                subject: '영어',
                score: 50
            }
        ]);
        console.log(result);
    }
}

interface ExamResult {
    subject: string;
    score: number;
}

interface ExamReport {
    doAlgorithm(data: ExamResult[]): ExamResult[];
}

class OrderOfGoodGrades implements ExamReport {
    public doAlgorithm(data: ExamResult[]): ExamResult[] {
        return data.sort((a, b) => {
            return b.score - a.score    // 내림차순
        });
    }
}

class OrderOfBadGrades implements ExamReport {
    public doAlgorithm(data: ExamResult[]): ExamResult[] {
        return data.sort((a, b) => {
            return a.score - b.score    // 오름차순
        });
    }
}

const context = new Context(new OrderOfGoodGrades());
console.log('Order of good grades : ');
context.doSomeBusinessLogic();
console.log('--');

console.log('Order of bad grades : ');
context.setStrategy(new OrderOfBadGrades());
console.log('--');
context.doSomeBusinessLogic();
```

### 장점
1. 알고리즘을 사용하는 코드에서 알고리즘의 구현 세부 정보를 분리할 수 있다.

### 단점
1. 패턴과 알고리즘이 별로 없는 경우 오히려 코드가 복잡해질 수 있다.


<br>

## 옵저버 패턴 (Observer pattern)
주체가 어떤 객체(subject)의 상태 변화를 관찰하다가 상태 변화가 있을 때마다 옵저버들에게 변화를 알려주는 디자인 패턴이다.  
주체란 객체의 상태 변화를 보고 있는 관찰자이며, 옵저버들이란 이 객체의 상태 변화에 따라 전달되는 '추가 변화 사항'이 생기는 객체들을 의미한다.  
옵저버 패턴은 이벤트 기반 시스템에 사용하며 MVC 패턴에도 사용된다.


<br>

## 프록시 패턴 (Proxy pattern)
대상 객체(subject)에 접근하기 전 그 접근에 대한 흐름을 가로채 대상 객체 앞단의 인터페이스 역할을 하는 디자인 패턴이다.  
이를 통해 객체의 속성, 변환 등을 보완하며 보안, 데이터 검증, 캐싱, 로깅에 사용한다.


<br>

## 이터레이터 패턴 (Iterator pattern)
이터레이터(iterator)를 사용하여 컬렉션(collection)의 요소들에 접근하는 디자인 패턴이다.  
이터레이터를 통해 순회할 수 있는 여러 가지 자료형의 구조와는 상관없이 이터레이터라는 하나의 인터페이스로 순회가 가능하다.


<br>

## 노출모듈 패턴 
즉시 실행 함수를 통해 private, public 같은 접근 제어자를 만드는 패턴을 말한다.  
자바스크립트는 접근 제어자가 존재하지 않고 전역 범위에서 스크립트가 실행되기 때문에 노출모듈 패턴을 통해 private와 public 접근 제어자를 구현할 수도 있다.


<br>

## MVC 패턴
모델(Model), 뷰(View), 컨트롤러(Controller)로 이루어진 디자인 패턴이다.
애플리케이션의 구성 요소를 세 가지 역할로 구분하여 개발 프로세스에서 각각의 구성 요소에만 집중해서 개발할 수 있다.  
재사용성과 확장성의 용이하다는 장점이 있고, 애플리케이션이 복잡해질수록 모델과 뷰의 관계가 복잡해지는 단점이 있다.  
MVC 패턴을 이용한 대표적인 라이브러리는 리액트가 있다.

### 모델 (Model)
애플리케이션의 데이터인 데이터베이스, 상수, 변수 등을 뜻한다.
뷰에서 데이터를 생성하거나 수정하면 컨트롤러를 통해 모델을 생성하거나 갱신한다.

### 뷰 (View)
사용자에게 보일 사용자 인터페이스를 담당한다. 

모델을 기반으로 사용자가 볼 수 있는 화면이다.
모델이 가지고 있는 정보를 따로 저장하지 않아야 하며 단순한 사각형 모양 등 화면에 표시하는 정보만 가지고 있어야 한다. 또한, 변경이 일어나면 컨트롤러에 이를 전달해야 한다.

### 컨트롤러 (Controller)
하나 이상의 모델과 하나 이상의 뷰를 잇는 다리 역할을 하며 이벤트 등 메인 로직을 담당한다.
모델과 뷰의 생명주기도 관리하며, 모델이나 뷰의 변경 통지를 받으면 이를 해석하여 각각의 구성 요소에 해당 내용에 대해 알려준다.


<br>

## MVP 패턴
MVC 패턴으로부터 파생되었으며 컨트롤러가 프레젠터(presenter)로 교체된 패턴이다.
뷰와 프레젠터는 일대일 관계이기 때문에 MVC 패턴보다 더 강한 결합을 지닌 디자인 패턴이다.


<br>

## MVVM 패턴
MVC의 컨트롤러가 뷰모델(view modal)로 바뀐 패턴이다.  
뷰모델은 뷰를 더 추상화한 계층이며, 커맨드와 데이터 바인딩을 가지는게 특징이다.  
뷰와 뷰모델 사이의 양방향 데이터 바인딩을 지원하며 UI를 별도의 코드 수정 없이 재사용할 수 있고 단위 테스팅하기 쉽다는 장점이 있다. 대표적인 프레임워크로 뷰가 있다.


<br>

# 프로그래밍 패러다임
프로그래밍의 관점을 갖게 해주는 역할을 하는 개발 방법론이다.

## 선언형
**'무엇을'** 풀어내는가에 집중하는 패러다임이다.

## 함수형 프로그래밍
선언형 패러다임의 일종으로, 작은 '순수 함수'들을 블록처럼 쌓아 로직을 구현하고 '고차 함수'를 통해 재사용성을 높인 프로그래밍 패러다임이다.  
자바스크립트는 단순하고 유연한 언어이며, 함수가 일체 객체이기 때문에 객체지향 프로그래밍보다는 함수형 프로그래밍 방식이 선호된다.

### 순수 함수
출력이 입력에만 의존하는 것을 의미한다

### 고차 함수
함수가 함수를 값처럼 매개변수로 받아 로직을 생성할 수 있는 것을 뜻한다

### 일급 객체
고차 함수를 쓰기 위해 해당 언어가 일급 객체라는 특징을 가져야 한다.
- 변수나 메서드에 함수를 할당할 수 있다
- 함수 안에 함수를 매개변수로 담을 수 있다
- 함수가 함수를 반환할 수 있다


<br>

## 객체지향 프로그래밍 (OOP)
객체들의 집합으로 프로그램의 상호 작용을 표현하며 데이터를 객체로 취급하여 객체 내부에 선언된 메서드를 활용하는 방식이다. 설계에 많은 시간이 소요된다.

### 특징
1. 추상화
    - 복잡한 시스템으로부터 핵심적인 개념 또는 기능을 간추려내는 것
2. 캡슐화
    - 객체의 속성과 메서드를 하나로 묶고 일부를 외부에 감추어 은닉하는 것
3. 상속성
    - 상위 클래스의 특성을 하위 클래스가 이어받아서 재사용하거나 추가, 확장하는 것
4. 다형성
    - 하나의 메서드나 클래스가 다양한 방식으로 동작하는 것

### 설계 원칙
1. 단일 책임 원칙 (S)
    - 모든 클래스는 각각 하나의 책임만 가져야 하는 원칙
2. 개방-폐쇄 원칙 (O)
    - 유지 보수 사항이 생긴다면 코드를 쉽게 확장할 수 있도록 하고 수정할 때는 닫혀 있어야 하는 원칙
3. 리스코프 치환 원칙 (L)
    - 프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 하는 것을 의미
4. 인터페이스 분리 원칙 (I)
    - 하나의 일반적인 인터페이스보다 구체적인 여러 개의 인터페이스를 만들어야 하는 원칙을 말합니다.
5. 의존 역전 원칙 (D)
    - 자신보다 변하기 쉬운 것에 의존하던 것을 추상화된 인터페이스나 상위 클래스를 두어 변하기 쉬운 것의 변화에 영향받지 않게 하는 원칙


<br>

## 절차형 프로그래밍
로직이 수행되어야 할 연속적인 계산 과정으로 이루어진 방식이다.  
일이 진행되는 방식이 그저 코드를 구현하기만 하면 되기 때문에 코드 가독성이 좋으며 실행 속도가 빠르다. 그렇기 때문에 계산이 많은 작업 등에 쓰인다.  
단점으로는 모듈화하기가 어렵고 유지 보수성이 떨어진다는 점이다.


<br>

# 요약


<br>

# 예상 면접 질문


<br>

# 참고 문서
[싱글톤 패턴이란](https://tecoble.techcourse.co.kr/post/2020-11-07-singleton/)  
[refactoring.guru](https://refactoring.guru/design-patterns)