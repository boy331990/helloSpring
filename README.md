# helloSpring
study spring from inflearn

========== 프로젝트 환경설정 Start ==========
1. 프로젝트 생성
   1. IntelliJ 설치
   2. https://start.spring.io를 이용 기본 프로젝트 생성
   3. IntelliJ Gradle 대신 자바 직접 실행으로 변경
      1. Preferences -> Gradle 검색 -> Build and run using, Run tests using: Gradle -> IntelliJ IDEA로 변경

2. 라이브러리 살펴보기
   1. 스프링 부트 라이브러리
      1. spring-boot-starter-web
         1. spring-boot-stater-tomcat: 톰캣(웹서버)
         2. spring-webmvc: 스프링 웹 MVC
      2. spring-boot-starter-thymeleaf: 타임리프 템플릿 엔진(View)
      3. spring-boot-starter(공통): 스프링 부트 + 스프링 코어 + 로깅
         1. spring-boot
            1. spring-core
         2. spring-boot-starter-logging
            1. logback, slf4j
   2. 테스트 라이브러리
      1. spring-boot-starter-test
         1. junit: 테스트 프레임워크
         2. mockito: 목 라이브러리
         3. assertj: 테스트 코드를 좀 더 편하게 작성하게 도와주는 라이브러리
         4. spring-test: 스프링 통합 테스트 지원

3. View 환경설정
   1. Welcome Page 만들기
      1. resources/static/index.html 생성
         1. static/index.html을 올려두면 Welcome page 기능을 제공한다.
   2. thymeleaf 템플릿 엔진
      1. thymeleaf 공식 사이트: https://www.thymeleaf.org/
      2. 스프링 공식 튜토리얼: https://spring.io/guides/gs/serving-web-content/
      3. 스프링부트 메뉴얼: https://docs.spring.io/spring-boot/docs/2.3.1.RELEASE/reference/ html/spring-boot-features.html#boot-features-spring-mvc-template-engines
   3. thymeleaf 템플릿엔진 동작 확인
      1. Controller 생성
         1. main/java/hello/hellospring/controller/HelloController.java
      2. hello.html 생성
         1. resources/templates/hello.html
      3. localhost:8080/hello 접속/확인
   4. 컨트롤러에서 리턴 값으로 문자를 반환하면 뷰 리졸버(viewResolver)가 화면을 찾아서 처리한다.
      1. 스프링 부트 템플릿엔진 기본 viewName 매핑
      2. resources:templates/ + {ViewName} + .html
* spring-boot-devtools 라이브러리를 추가하면, html 파일을 컴파일만 해주면 서버 재시작 없이 View 파일 변경이 가능하다.

4. 빌드하고 실행하기(콘솔로 이동)
   1. 생성된 프로젝트 디렉토리로 이동 후 ./gradlew build
   2. cd build/libs
   3. java -jar hello-spring-0.0.1-SNAPSHOT.jar
   4. localhost:8080 접속하여 확인
========== 프로젝트 환경설정 End ==========

========== 스프링 웹 개발 기초 Start ==========
1. 정적 컨텐츠
   1. resources/static 하위에 위치하는 html을 찾아 표시한다.

2. MVC와 템플릿 엔진
   1. MVC : Model, View, Controller
      1. 내장 톰켓 서버 
      2. Controller 
      3. viewResolver (Spring Boot 기본 내장, resources/templates에 위치)
      4. Thymeleaf 템플릿 엔진 처리

3. API
   1. @ResponseBody 어노테이션을 붙인다.
      1. 해당 어노테이션은 return시 viewResolver가 아닌 HttpMessageConverter가 동작한다.
         1. 기본 문자처리 : StringHttpMessageConverter
         2. 기본 객체처리 : MappingJackson2HttpMessageConverter
         3. byte 처리 등등 기타 여러 HttpMessageConverter가 기본으로 등록되어 있음.
      2. 반환 데이터 타입은 대체로 JSON타입
========== 스프링 웹 개발 기초 End ==========

========== 회원 관리 예제 - 백엔드 개발 Start ==========

========== 회원 관리 예제 - 백엔드 개발 End ==========