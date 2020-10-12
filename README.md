# SpringTutorial

## 1. 프로젝트 생성

- 준비물 및 도구
    - Java 11
    - IntelliJ IDEA CE
    - Gradle

- 스프링 부트 스타터 사이트에서 프로젝트 생성

    [https://start.spring.io](https://start.spring.io/)

    - Project → Gradle
    - Spring Boot → 2.3.x
    - Language → Java
    - Packaging → jar
    - groupId → com.example
    - artifactId → spring-tutorial
    - dependencies
        - Spring-Web
        - Tyhmeleaf (HTML 탬플릿 엔진)

- IntelliJ IDEA의 build and run을 Gradle에서 IntelliJ IDEA로 바꾸기
    - IntelliJ IDEA가 최신 버전에서 Gradle을 default 실행자로 사용하는데 IDEA 보다 실행 속도가 느림

    - Preferences → Build, Execution, Deployment → Build Tools → Gradle
        - Build and run using: Gradle → IntelliJ IDEA
        - Run tests using: Gradle → IntelliJ IDEA

- 동작 확인

    Spring Boot Application 클래스 실행 후, [localhost](http://localhost):8080 들어가서 에러페이지 확인

## 2. 라이브러리 살펴보기

Gradle은 의존관계가 있는 라이브러리를 함께 다운로드 한다.

→ 첫 프로젝트 생성 시 의존성을 Spring-Web, Thymeleaf 2가지만 설정했어도, Spring 애플리케이션에 동작하기 위한 의존 관계의 라이브러리가 함께 다운로드 된다.

- 스프링 부트 라이브러리
    - spring-boot-starter-web
        - spring-boot-starter-tomcat: 톰캣 내장 웹서버
        - spring-webmvc: 스프링 웹 MVC
    - spring-boot-starter-thymeleaf: HTML 템플릿 엔진(View)
    - spring-boot-starter (공통 의존 관계)
        - spring-boot
            - spring-core
        - spring-boot-starter-logging (실제 현업에서 로그 남기는 것에 관련된 라이브러리)
            - logback, slf4j
- 테스트 라이브러리
    - spring-boot-starter-test
        - junit: 테스트 프레임워크
        - mockito: 모킹 라이브러리
        - assertj: 테스트 코드 작성을 좀 더 편하게 도와주는 라이브러리
        - spring-test: 스프링 통합 테스트 지원

### Spring boot 메뉴얼과 튜토리얼 참고하기

- 스프링 공식 튜토리얼: [https://spring.io/guides/gs/serving-web-content/](https://spring.io/guides/gs/serving-web-content/)
- 스프링 부트 메뉴얼: [https://spring.io/projects/spring-boot#learn](https://spring.io/projects/spring-boot#learn)

## 3. Welcome Page 만들기

- Spring boot의 Welcome Page

    resources:static/index.html 파일이 웰컴 페이지 역할을 수행

    ```jsx
    <!DOCTYPE HTML>
      <html>
      <head>
          <title>Hello</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" /> </head>
      <body>
      Hello
      <a href="/hello">hello</a>
      </body>
      </html>
    ```

- thymeleaf 템플릿 엔진

    html을 베이스로 하되, for loop와 같은 기능을 통해 html 파일을 생성할 수 있도록 해줌

- Hello Page
    - Hello Controller

    ```jsx
    @Controller
    public class HelloController {

    	@GetMapping("hello") // [GET]http://host/hello 으로 맵핑
    	public String hello(Model model) {
    		model.addAttribute("data", "hello"); // data 애트리뷰트의 값은 hello
    		return "hello"; // resources:templates/hello.html 파일을 렌더링
    	}
    }
    ```

    - hello.html

    ${변수명} 은 렌더링 해주는 Controller에서 addAttribute 메소드를 통해 모델에 설정된 속성 값

    ```jsx
    <!DOCTYPE HTML>
    <html xmlns:th="http://www.thymeleaf.org"> <head>
          <title>Hello</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" /> </head>
    <body>
    <p th:text="'안녕하세요. ' + ${data}">
    	안녕하세요. 손님
    </p>
    </body>
    </html>
    ```

    - thymeleaf 템플릿 엔진 동작 확인

        실행: http://localhost:8080/hello

        [동작 환경 그림]

        ![그림](./image/템플릿엔진동작그림.png)

        - Controller의 return String → viewResolver 가 화면을 랜더링
            - 스프링 부트에서 기본적인 탬플릿 엔진의 View Name 맵핑 구조

                return {viewName:String};

                → `resources:templates/`{viewName}`.html`

### Spring Boot Devtools

- 스프링 부트 앱의 재시작 없이 html 리컴파일로 View 변경사항의 실시간 적용을 가능하게 하는 라이브러리
    - IntelliJ IDEA에서 html 리컴파일: 메뉴 → build → Recompile


## 4. 빌드 및 실행

터미널에서 다음의 명령어를 실행

```jsx
// 프로젝트의 루트 폴더에 위치한다고 가정
// 프로젝트 빌드
./gradlew build

// 빌드된 jar 파일이 있는 곳으로 이동
cd ./build/libs

// java로 빌드된 jar파일 실행
java -jar 프로젝트이름-x.x.x-SNAPSHOT.jar //빌드에 따른 버전 다름
```

빌드 클리닝

```jsx
// build 폴더 지우기
./gradlew clean

// build 폴더 싹 지운 후, 다시 빌드
./gradlew clean build
```

## 스프링 웹 개발 기초

### 1. 정적 컨텐츠

스프링 부트 컨트롤러에서 별도의 조작이 없으며, 클라이언트로부터 리퀘스트가 오자마자 바뀌지 않는 정적 자원(파일)을 클라이언트로 즉시 반환해주는 서버 리스폰스 형식

h`resources:static/` 밑에 파일을 위치시키면, 스프링 부트에서는 각 파일의 이름으로 정적컨텐츠를 인식

- `resources:static/hello-static.html`

```jsx
<!DOCTYPE HTML>
  <html>
  <head>
      <title>static content</title>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8" /> </head>
<body>
정적 컨텐츠 입니다.
  </body>
</html>

```

실행: http://localhost:8080/hello-static.html

[정적 컨텐츠 동작 원리]

![그림](./image/정적컨텐츠동작그림.png)

- 클라이언트로 부터 요청이 올 때,
    1. 먼저, 스프링 부트에 해당 요청을 처리하는 컨트롤러가 있는지 검사
    2. 없다면 static 폴더 내부의 정적 컨텐츠를 확인하여 클라이언트 리스폰스로 보내줌