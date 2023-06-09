# WIL9

이번장은 자바 문법에 관한 이야기다.

## **1. 람다와 스트림**

**람다란?**

람다(lambda)란 이름이 없는, 즉 익명을 의미한다. 따라서 람다함수란 익명함수를 의미하는데 말그대로 익명함수이기에 함수의 이름이 없이 괄호와 매개변수, 화살표(->)만으로 함수를 작성할 수 있다. 다음과 같이 말이다.

```
(x) -> x+1
```

이렇게 코드를 작성하면 코드가 간결해진다는점, 개발자의 의도가 명확히 드러난다는 점, 함수를 선언과 동시에 사용할 수 있어 생산성이 높아진다는점, 병렬처리의 가능하다는 장점이 있다. 이러한 이유로  java8에서부터 람다 함수를 지원한다.

하지만, 장점만 존재하는 것은 아니다.

람다를 사용하면, 다음과 같은 단점이 존재하기에 주의해서 사용해야한다. 첫째, 함수 이름이 없기 때문에 반복된 작업을 간결히 하기 위해 작성된 본래 함수의 목적과 달리, 람다 함수는 재사용이 불가능하다. 둘째, 편리하다고 불필요하게 마구잡이로 사용하면 외려 코드의 가독성을 떨어뜨린다. 이외에도 디버깅이 어렵다는 점, 재귀로 만들기 부적합하다는 점 등의 단점이 있으니 주의해서 사용해야한다.

**스트림이란?**

스트림은 java8에서 새롭게 추가된 기능으로, 데이터의 흐름을 의미한다. 이를 활용해 데이터를 활용하고 처리하는 작업을 할 수 있는데 대표적인 예로는 배열을 병렬처리 할 수 있게 해준다. 기존에 배열을 반복문으로 돌면서 원소에 접근할때는, **for, foreach**를 사용해 원소에 하나씩 접근했다. 원소의 개수가 적다면, 상관이 없지만, 원소가 많고 코드가 복잡해질 수 있다. 반면, 스트림은 내부적으로 람다식을 활용해 배열 병렬처리를 가능하게 한다. 다음과 같은 방법을 통해서 말이다.

```
stream.forEach(num->System.out.println(num));
```

## **2. Optional**

**Optional이란?**

자바에서 가장 많이 발생하는 에러는 NullPointerException에러이다. 이 에러는, 존재해야할 객체에 Null값이 들어가 있어 발생하는 에러로 이를 피하기 위해선 코드를 작성할 때 반드시 null 여부를 검사해줘야한다.

하지만, 매번 null여부를 검사하게 되면, if - else문을 중첩으로 사용하게 되고 코드의 가독성이 떨어진다. 따라서, 이러한 문제를 해결하고자 java8에서는 null이 올 수도 있는 값을 감싸는 wrapper 클래스로 Optional을 추가로 등장시켰다.

다음과 같은 방법으로 Optional 객체를 표현할 수 있다.

```
Optional<String> optional
```

Optional의 등장으로 null이 존재하는 경우와 같이 객체가 존재하지 않는다면, 이를 null로 반환하는 것이 아니라 명시적으로 이 객체가 비어있음을 의미하는 Optional.empty()로 표현한다.

객체가 비어있음 여부는 optional.isPresent()라는 메소드를 통해 확인할 수 있다. 객체가 비어있다면 False를 존재한다면 True를 반환한다.

또한, 객체가 null이 가능하거나 절대 가능하지 않은 여부에 따라서 선언도 바뀔 수 있다.

null이 불가능한경우엔 **Optional.of(~~)**를 사용하고

반대로 null이 가능하다면 Optional.ofNullable()로 사용할 수 있다.