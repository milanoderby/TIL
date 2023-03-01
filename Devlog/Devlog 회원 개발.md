# Devlog 회원 개발

## 기술 스택 및 기술적 요구사항 정의

Project Build: Gradle - Kotlin

Language: Kotlin

Spring Boot Version: 2.7.9

Packaging: Jar

JDK Version: 11

<br>

### 의존성 선택

https://start.spring.io/ 기준으로 설명합니다.

Spring Boot 버전은 JDK 11버전과 호환되는 최신 안정화버전(GA)인 2.7.9 버전을 사용합니다.

3.0.3 버전부터는 JDK17이 기본 설정 되어있습니다.

<br>

### BE

- Spring Web - REST 및 MVC

- Spring Data JPA

- H2 Database

- Spring Configuration Processor

<br>

### FE

SSR(Server Side Rendering)을 사용하여 FE를 제공합니다.

- Spring Boot DevTools
- Thymeleaf

<br>

## 회원 정책 및 기능상 요구사항 정의

### 회원가입

회원을 가입하고 조회할 수 있다.

<br>

회원 도메인

- 회원번호 (자동 생성)
- ID
- 비밀번호
- 이름
- 생년월일
- 이메일

<br>

#### 회원 도메인을 아래와 같이 표현할 수 있어야한다.

1. 회원 도메인 협력관계도 - 기획자도 볼 수 있는 그림 (각 객체는 역할/책임별로 나눕니다.)
2. 클래스 다이어그램 - 
3. 객체 다이어그램

<br>

#### 구현방법

1. Member 객체 생성
   - 김영한님은 long 대신에 Long을 사용했는데, 이유는 모릅니다.
2. MemberRepository 인터페이스 생성
   - 회원 저장
   - 회원 조회
3. MemberRepository 구현체 생성 - MemoryMemberRepository 생성
4. MemberService 인터페이스 생성
   - 회원 가입
   - 회원 찾기
5. MemberService 구현체 생성 - MemberServiceImpl 생성

<br>

### IntelliJ 단축키

ctrl + shift + enter: 자동완성 및 세미콜론까지 붙여줍니다.

ctrl + alt + v: new Member 까지 입력한 후, 입력하면 변수를 생성해줍니다.

ctrl + shift + t: Test클래스를 생성해줍니다. -> 단순히 테스트 패키지에 클래스를 생성합니다.

ctrl + alt + m: 메소드 내의 중복된 코드 부를 메소드로 추출합니다.

ctrl + r: 최근에 실행한 모드(Run 또는 Debug)로 코드를 실행합니다.

<br>

### Test 코드 생성

1. given: 테스트할 객체를 줍니다.
2. when: 테스트할 상황을 줍니다.
3. then: 테스트 결과와 예상 결과를 비교합니다.
4. @DisplayName: Junit5부터 지원하는 기능으로 test 메소드에 붙이는 애노테이션으로 단위 테스트 수행 시, 노출되는 기능명입니다.

<br>