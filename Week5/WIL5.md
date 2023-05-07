# WIL5

## **1. 객체지향 패러다임과 객체란?**

**객체지향 패러다임**

앞서 언급한 절차지향형 프로그래밍에서 이전까지 프로그래밍의 패러다임은 함수지향적이었다. 코드를 논리적인 단위로 구분하는 방식으로 작성하며 인간의 입장에서 기계를 이해하고자 하는 방향의 언어들이 주를 이루었다. 하지만, 객체지향 패러다임은 이와 다르게 현실세계 그 자체를 인지하고 프로그래밍에 반영하도록 하고자 하는 패러다임이다. 즉, 세상에 존재하는 모든 사물들을 인지하고 모든 사물을 객체화하고자한다. 따라서 각각의 사물의 고유한 속성과 행동을 정의하고 이를 Attribute와 Operation으로 정의하고자한다.

**객체란?**

앞서 Class와 Object를 구분할때 클래스는 Definition 특정 사물을 정의한 것을 의미하고 객체는 이를 실체화(instance화) 한 것을 의미했다. 객체지향 패러다임에서의 객체는

## **2. 객체지향 4대 특성**

### **캡슐화 (Encapsulation)**

객체지향 프로그래밍에선, Object끼리 서로 의사소통을 해야하는데 이를 Message-Passing이라고 한다. 서로가 서로의 데이터를 주고 받아야하는데 이때 내가 알고 싶은 객체에 대한 모든 정보를 다 알아야할까? 아니다. 내가 얻고 싶은 정보만 얻으면 된다. 반대로 내가 상대방에게 데이터를 제공할때 내부 구현 코드와 같이 세부적인 정보까지 다 공개할 필요가 있을까? 이 또한 아니다.

따라서, 객체지향에서는 필요한 최소한의 부분만 Interface를 통해 노출하고 나머지 detail한 부분은 철저하게 은닉하는데 이를 Encapsulation이라고 한다.

### **상속 (Inheritance)**

여러 객체들은 각기 다른 고유한 객체이면서도 비슷한 성질을 띈다. 따라서, 비슷한 객체들의 공통된 속성을 추출해 이를 부모 클래스인 SuperClass로 정의하는 방식을 Generalization이라고 하고 SuperClass를 부모 클래스, 기존 클래스들을 SubClass인 자식 클래스라하며 이 객체들간의 관계를 상속이라고 표현한다.

이렇게 부모 클래스와 자식 클래스를 정의하고 나면 자식 클래스는 부모 클래스의 모든 속성들을 가질 수 있다. 물론, 자식 클래스는 자기 자신에 맞는 개인적인 속성들을 추가할 수 있다.

### **추상화(Abstraction)**

현실 세계의 사물에서 내가 프로그래밍하고 싶은 속성과 행동을 추출하는 것을 의미한다.

현실 세계의 사물인 사람, 학생에 대한 이야기를 해보자. 내가 만들고 싶은 프로그램이 클래스넷, 헤이영캠퍼스와 같이 학생 정보를 관리하는 시스템이라면, 학생에게서 학번, 이름, 성적과 같은 필요 정보를 추출하고 평점 계산과 같은 필요 행동(함수)를 추출하면 된다. 그러나, 내가 동아리 관리 시스템을 만들고 싶다면? 학생에게서 추출해야하는 정보는 관련 분야 경력과 경험등을 추출하면 된다. 이렇게

추상화라는 말을 한국말로 생각하면 다소 이해가 어렵다. 따라서, 특정 객체에서 내가 만들 프로그램에 맞게 **필요한 것을 추출하는 것을**  추상화로 이해하도록하자.

### **다형성(Polymorphism)**

하나의 메시지에 대해 서로 다른 방법으로 응답할 수 있는 기능을 의미한다. 부모 클래스에게 상속 받은 메소드 하나를 자식 클래스에서 다양한 방법으로 구현할 수 있다. 가령 자동차라는 부모클래스(Interface)의 엔진작동(Operation)을 자식클래스인  전기차, 내연기관차가 상속받았다고 하자. 엔진을 작동한다는 의미는 공통적이나 전기차, 내연차의 세부 작동 방식은 다르다. 이와 같이 엔진작동이라는 메시지를 전기, 내연 각각 다른 방식으로 응답하는 것을 다형성이라고 한다.

이를 지원하는 방법으론 오버라이딩이 존재하는데 이는 아래에서 추가적으로 언급하겠다.

## **3. 오버라이딩 vs 오버로딩**

### **오버라이딩**

부모클래스에서 상속받은 메소드를 자식 클래스에서 재정의하는 것을 의미함. 위의 자동차의 예시와 같이 부모 클래스에서 '엔진작동'이라는 메소드를 상속했으나 이를 자식 클래스 각자의 방법으로 정의하는 것을 오버라이딩이라고 함.

이는 단순 재정의하는 행동이기에 자식 클래스에서 오버라이딩하고자 하는 메소드의 이름, 매개변수, 리턴 값이 모두 같아야 한다.

### **오버로딩**

한 클래스내에 같은 이름을 가진 메소드가 존재하더라도, 이 메소드의 파라미터의 타입과 개수, 반환 타입에 따라 같은 이름의 메소드를 여러개 정의하는 것을 의미함.

(단, 이때 리턴 타입만 다른 것은 안되고 파라미터의 속성이 달라야한다.)

# **4. call by value vs call by reference**

어떤 변수가 선언될때 해당 변수는 메모리공간상에서 메모리를 할당받는다. 이 메모리의 위치를 메모리의 '주소 또는 참조(Reference)라고 한다. 이 변수가 특정 값을 가지고 초기화 될때 해당 메모리 위치에 값, value가 저장된다.

즉, 어떤 메모리 공간의 주소는 고유하지만, 여러 메모리 공간 주소에 특정 값은 여러번 저장될 수 있다.

이 개념을 확장해서 함수가 호출될때를 생각해보자. 함수가 생성되면 메모리의 스택 영역에선 함수를 위한 별도의 공간을 할당해준다.

### **Call by value**

이 때 Call by value는 해당 함수의 정의된 값 자체를 복사한 복사본을 넘겨준다.

### **Call by Reference**

어떤 함수를 호출할때 해당 함수의 주소값 자체를 전달한다. 즉, Call by value와는 달리 복사본이 아닌 원본 자체, 해당 함수가 정의된 공간 자체를 전달해서 필요한게 있으면 그것을 직접 찾아가서 참고하라는 것을 의미한다.