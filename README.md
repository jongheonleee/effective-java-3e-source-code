# 🚀 Effective Java

## 📌 02. 객체 생성과 파괴 

<br>
<br>

### 📦 Item 01. 생성자 대신 정적 팩터리 메서드를 고려하라

> ### 무분별하게 new 생성자()로 객체를 생성하는 것을 지양, 정적 팩터리 메서드 활용하여 자원 효율적으로 사용, 객체 적극적으로 재사용

- 정적 팩터리 메서드 : static 팩토리 메서드를 의미 
- new 연산자를 통한 객체 생성은 자원을 낭비함, new 를 사용하면 heap영역에 항상 메모리 공간이 확보됨
- 따라서, 객체를 효율적으로 생성하기 위해서 정적 팩터리 메서드 사용 

- 장점
  - (1) 이름을 갖음 - 메서드 여러개 만들 수 있음
  - (2) 인스턴스 재사용 - 불필요하게 인스턴스를 반복해서 생성할 필요 없이 재사용할 수 있음(flyweight 패턴)
  - (3) 다형성, 공변이 가능함 - 반환할 클래스를 자유롭게 선택 가능. 예를 들어, Car car = CarFactory.getInstance("sportCar");
  - (4) 매개변수에 따라 다른 객체 반환 -  CarFactory.getInstance("truck"), CarFactory.getInstance("sportCar"), ...
  - (5) 정적 팩터리 메서드를 작성할 때, 반환 객체 클래스가 존재하지 않아도됨 - Future 패턴과 유사(빈 박스제공)

- 단점
  - (1) 상속 불가능 - 생성자가 private 이기 때문
  - (2) 메서드 찾기 어려움 - 이름이 여러개임
<br>
  
### 📦 Item 02. 생성자에 매개변수가 많다면 빌더를 고려하라

> ### iv가 많고 객체 생성시에 매개변수가 많은 경우, 생성자나 정적 팩터리 메서드보단 빌더 패턴을 활용

- 빌더 패턴 : "생성자에 매개변수가 많음 -> 생성자 오버로딩 많이함, 매개변수 순서 혼동" 해결하기 위해 내부적으로 빌더 클래스를 정의해서 해당 클래스를 생성할 때 사용

<img src="https://github.com/jongheonleee/effective_java/assets/87258372/8dff7ae4-134d-40c5-ba3f-00a8c2292e35" width="500" height="500"/>

<img src="https://github.com/jongheonleee/effective_java/assets/87258372/163df800-b0bc-490e-8828-9e1f0491d3e0" width="500" height="500"/>
<br>

### 📦 Item 03. private 생성자나 열거 타입으로 싱글턴임을 보증하라 

> ### 열거형으로 싱글톤 만드는 방식이 가장 좋음

- 싱글톤 : 인스턴스 하나만 생성하고 공유
- 구현 방법 - (1) public static final -> (2) 정적 팩터리 메서드 -> (3) 열거형
<br>

### 📦 Item 04. 인스턴스화를 막으려거든 private 생성자를 사용하라 

> ### private 생성자를 통해 인스턴스화 막음 

- 굳이 이렇게 까지 할 필요는 없음, 물론 싱글톤같은 디자인 패턴에는 적용해야함
<br>

### 📦 Item 05. 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라

> ### 인스턴스 생성시 필요한 자원을 넘겨줌, 메서드로 넘겨줄 수도 있음(sort("비교대상", "비교기준"))

- 전략 패턴, 알고리즘을 외부에서 통째로 넣어줌 
- 예시에서 나온 맞춤법 검사기의 문제점은 "자주 변경될 부분이 구체적으로 작성되어 있음"
- 예를 들어, 언어마다 맞춤법이 다르기 때문에 해당 부분을 쉽게 변경할 수 있게 구현해야함(전략 패턴)

<img src="https://github.com/jongheonleee/effective_java/assets/87258372/826a7a71-0ef9-4632-9f13-7ece2133d472" width="500" height="500"/>
<br>


### 📦 Item 06. 불필요한 객체 생성을 피하라 

> ### 재사용 가능한 객체는 공유해서 사용하자

- 멀티 쓰레드 환경도 고려해야함
- 재사용 가능한 객체라는 의미는 공유 가능한 정보를 갖고 있냐도 포함임
- 즉, 내부 iv가 불변이 아닌 경우, 또한 동기화 처리가 안되있는 경우는 멀티 쓰레드 환경에서 공유해서 사용할 경우 프로그램에 혼동을 야기할 수 잇음
<br>

<img src="https://github.com/jongheonleee/effective_java/assets/87258372/a8a0f7a2-7aa3-46ee-9171-bb128b548d31" width="500" height="500"/>

### 📦 Item 07. 다 쓴 객체 참조를 해제하라!

<img src="https://github.com/jongheonleee/effective_java/assets/87258372/906d2be7-61a3-4cfc-bb66-3fb42725211a" width="500" height="500"/>

<img src="https://github.com/jongheonleee/effective_java/assets/87258372/5cc3685f-8981-40e4-91ac-69bfa46564ca" width="500" height="500"/>


- 가비지 컬렉터가 사용되지 않는 객체를 회수하지만, 100% 회수해주지 않음
- 참조가 걸려있지만 사용하지 않는 경우에는 회수하지 못함
  - 대표적으로 Flyweight 패턴에서 맵에 저장된 안쓰는 객체 회수 못함
- 위와 같은 경우는 개발자가 참조를 해제해야함 



### 📦 Item 08. finalizer와 cleaner 사용을 피하라
<br>

### 📦 Item 09. try-finally 보다는 try-with-resources를 사용하라
<br>

## 📌 03. 모든 객체의 공통 메서드 

<br>
<br>

### 📦 Item 10. equals는 일반 규약을 지켜 재정의하라 

<img src="https://github.com/jongheonleee/effective_java/assets/87258372/4703f7c8-5a32-492a-bc94-58be11616061" width="500" height="500"/>

- equals()를 오버라이딩 하지 않아야하는 상황
  - (1) 각 인스턴스는 고유함 : 기본적으로 참조 비교가 정상임
  - (2) 인스턴스의 '논리적 동치성'을 검사할 일이 없음 : 값 비교(논리적 동치성)를 의미함
  - (3) 조상에서 재정의한 equals()가 자손에서도 딱 들어맞는다 
    - 조상, 자손은 iv의 개수가 같거나 다름
    - 자손에서 iv가 추가되고 equals()를 오버라이딩 하면 문제가 발생
      - 동치관계(빈사성, 대칭성, 추이성, 일관성, null-아님) 충족 못함
      
<br>
      
- 양질의 equals()를 오버라이딩 하기 위한 규칙
  - (1) == 연산자를 사용해 입력이 자기 자신인지 확인
  - (2) instanceof 를 통해 타입 확인
  - (3) 대응되는 '핵심' 필드들이 모두 일치하는지 하나씩 확인 
  - (4) 대칭적? 추이성? 일관적? 스스로 자문 
  
<br>
