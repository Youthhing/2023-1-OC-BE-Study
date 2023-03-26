# Week1

정리해야할 주제가 많다. 입문편 강의는 이번이 3번째인데 3번을 듣고나서야 기본적인 이해가 되었다.

이번주 필수 정리주제는 다음과 같았다.

1. MVC 패턴이란?
2. API와 서버 - 아무 API나 써보길 권장
3. RestFul API에 대한 이해

### **1. MVC패턴이란?**

MVC 패턴은 소프트웨어 디자인 패턴 중 하나 Model View Controller의 약자로 사용자의 인터페이스와 데이터,논리 제어(비즈니스 로직)를 구분해서 구현하는데 주로 사용된다.

**Model**

APP이 사용할 데이터를 정의하고, 데이터를 Model 이라는 객체에 담아서 활용한다. 데이터가 변경되면 Model이 view에게 이를 알려 화면을 변경하거 컨트롤러에게 알리기도 한다.

**View**

데이터를 보여주는 방식을 의미한다. 주로 데이터를 어떻게 화면에 그리는지를 이야기하며 html 파일이 그 예이다.

**Controller**

사용자의 응답을 바탕으로 비즈니스 로직을 담당하는 역할이다. 사용자의 입력으로 데이터가 업데이트 되면 이를 처리한 후 업데이트된 모델을 뷰로 전달하는 역할을 한다.

![https://blog.kakaocdn.net/dn/LpG9D/btr5R1md7xO/ilXQAdEDwGhkCHz0aK82lk/img.png](https://blog.kakaocdn.net/dn/LpG9D/btr5R1md7xO/ilXQAdEDwGhkCHz0aK82lk/img.png)

위 그림을 예로 MVC 패턴에 대해 설명하자면 다음과 같다.

1) 웹 브라우저를 통해 사용자가 요청을 보낸다. (데이터를 포함)

2) Controller가 받은 응답을 기반으로 Model에 데이터를 담는다.

3) 로직을 처리해 return 값에 해당하는 템플릿을 viewResolver를 통해 찾고 해당 뷰에 모델과 함께 전달한다.

4) 뷰는 적절히 html로 변환 후 웹 브라우저에 데이터와 함께 화면을 보여준다.

### **2. API와 서버**

**API란?**

Application Programming Interface의 약자로 Application은 고유한 기능을 가진 모든 소프트웨어를, Interface는 두 소프트웨어 사이의 통신 규칙, 계약이라고 볼 수 있다. 즉, 프로그램과 프로그램 사이에서 데이터가 어떻게 통신는지를 정의한 규칙을 API라고 한다.

**장점**

API를 사용하면 내부 구현 방식을 알지 못하는 다른 프로그램과도 데이터를 주고 받을 수 있다.

가령, 내가 지도를 활용해 맛집 어플리케이션을 만든다고 하자. 이때 내가 일일히 지도에 대한 데이터를 넣어주는 것은 비효율적인 일이다. 이미 지도를 구현해놓은 카카오맵, 네이버지도 등이 있기 때문이다.

물론, 카카오맵, 네이버지도등의 내부 구현을 일일히 아는 것도 무의미할뿐더러 카카오, 네이버도 이를 공개하고 싶지 않을 것이다. 이 때 적절하게 데이터를 받아오기 위해 오픈 API를 활용할 수 있다.

기업들은 오픈API를 통해 부수입을, 개발자는 API를 활용해 개발 효율성을 높일 수 있다.

이는 공개 범위에 따라 오픈(퍼블릭) API, 파트너(특정 대상) API, 프라이빗 API로 구분된다.

**Spring에서 API를 구현하는 방식**은 다음과 같다.

@ResponseBody 어노테이션을 활용하는 것이다.

이전에 MVC 패턴을 활용할땐 데이터를 뷰를 통해 html로 화면에 전달했다.

그러나, API 방식을 활용해 Data를 바로 화면에 보낼 수 있다.

다음은, @ResponseBody 어노테이션을 활용한 예이다.

```
@GetMapping("hello-api")
    @ResponseBody
    public Hello HelloApi(@RequestParam("name") String name){
        Hello hello = new Hello();
        hello.setName(name);
        return hello;
    }

    static class Hello{
        private String name;

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }
    }
```

생성한 Hello객체를 통해 컨트롤러는 String이 아닌, 객체를 직접 반환한다.

객체는 JSON형태로 { key , value }의 쌍으로 직접 반환된다.

직접 반환한다는 의미는 HTTP(통신 프로토콜)의 Body 부분에 객체, 데이터를 직접 넣어준다는 것을 의미한다.

![https://blog.kakaocdn.net/dn/VIlDW/btr5UlktuQ2/ODJL5gqRmOkPHpaikwSy00/img.png](https://blog.kakaocdn.net/dn/VIlDW/btr5UlktuQ2/ODJL5gqRmOkPHpaikwSy00/img.png)

@ResponseBody 사용원리는 다음과 같다.

1) 사용자의 요청이 Controller에 도달한다. Controller는 mapping된 내용에 @ResponseBody의 존재를 확인한다.

2) @ResponseBody가 존재하면, HttpMessageConverter에서 데이터가 String이면 StringConverter를 통해 문자를 반환한다.

3) 데이터가 객체면 MaapingJackson2HttpMessageConverter를 통해 객체를 Json으로 변환 후 반환한다.

**서버**

서버란?

영어단어 Server에서 유래된 말로 서빙하는 사람. 즉 무언가를 가져다주는 컴퓨터를 의미한다.

컴퓨터가 네트워크를 통해 통신을 할 때 상호간의 요청이 존재하는데 요청을 보내는 노드 클라이언트, 요청을 받고 이에 대한 응답을 하는 노드를 서버라고 한다. 즉, 클라이언트에게 서비스를 제공하는 노드를 서버라고 한다.

서버의 역할은 어떤 서비스를 하느냐에 따라 결정된다. 브라우저의 요청을 처리하는 웹서버를 HTTP 서버라 하고, 메일을 주고받는 서버를 SMTP 서버라고 부를 수 있다.

웹 어플리케이션에서는 주로 HTTP 요청을 보내는데 그렇다면 프로그램의 요청의 종류는 무엇이 있을까?

HTTP 요청은 다음과 같이 4가지가 존재한다.

1. 읽기 : 페이지를 탐색해서 읽는다. --> Get

2. 쓰기 : 페이지에 글, 내용, 데이터를 등록한다. --> Post

3. 수정 : 존재하는 글, 데이터를 수정한다. --> Put

4. 삭제 : 존재하는 글을 삭제한다. --> Delete

이러한 요청들에 대해 적절한 리소스를 제공해주거나 로직을 처리해주는 것을 서버라고 한다.

### **3. RestFul API에 대한 이해**

**API의 문제**

어플리케이션 마다 API를 생성한다. 그러나, 규칙이 통일되어있지 않으니 프로그램마다 통신시 각자에 맞는 API를 작성해야함.

- -> API의 규칙을 표준화(통일)할 필요가 생김!

이러한 요구에 두 가지 표준화된 API가 등장했다.

**SOAP API**

Simple Object Access Protocol의 약자로 객체를 XML 형식을 통해 http 요청을 수신한다. 이는 하나의 프로토콜이다.

**REST API**

REST는 Respresentational State Transfer의 약자로, 이는 SOAP API와 다르게 프로토콜(규약)이 아닌 하나의 아키텍쳐이다. 사용자가 서버에 하는 요청은 극히 제한되어있는데 이를 CRUD라 기본적인 4가지 동작으로 작업을 구분해 Http 요청 다음과 같은 메소드 Get(조회), Post(생성), Put(수정), Delete(삭제)로 구분 요청을 처리하며 현재는 이 4가지보다 많은 기능을 제공하긴함.

RestAPI는 다음과 같은 제약을 지키며 설계되어야한다.

1. Stateless(무상태) 서버는 이전 모든 요청과 독립적으 클라이언트의 요청을 완료하여야한다.2. 계층화된 시스템서버와 클라이언트 사이에 다른 승인된 중개자가 들어올 수 있으며 서버는 다른 서버로 요청을 전달할 수도 있다.3. 캐시 가능성서버 응답 시간을 개선하기 위해 중개자에 일부 응답을 저장하는 프로세스이 캐싱을 지원한다.4. 온디맨드 코드

서버가 클라이언트에게 코드를 전송하여 클라이언의 기능을 일시적으로 확장하거나 지정할 수 있어야한다.

이러한 제약을 잘 지켜서 만든 API를 Restful하다고 하며 RestfulAPI라고 한다.

**단점**

API는 내 프로그램의 데이터를 다른 프로그램이 CRUD중 일부 기능을 수행할 수 있게 허용했다. 따라서, 보안과 관련되어 민감할 수 있다. 이를 보호하기 위해 두 가지 방법을 사용한다.

1. 인증 토큰 : 사용자에게 API를 호출할 수 있는 권한을 준다. 이 토큰을 통해 액세스 권한을 확인하 사용자를 구별한다.

2. API 키 : API를 호출하는 어플리케이션을 구분한다.

RestFul API에 대한 영상 하나를 추천하며 글을 마무리한다.

[https://tv.naver.com/v/2292653](https://tv.naver.com/v/2292653)