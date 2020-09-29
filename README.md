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
