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
1. 비즈니스 요구사항 정리
   1. 데이터 : 회원ID, 이름
   2. 기능 : 회원 등록, 조회
   3. 아직 데이터 저장소가 선정되지 않음(가상의 시나리오)
      1. 아직 데이터 저장소가 선정되지 않아, 우선 인터페이스로 구현 클래스를 변경할 수 있도록 설계
      2. 데이터 저장소는 RDB, NoSQL 등등 다양한 저장소를 고민중인 상황으로 가정
      3. 개발을 진행하기 위해 초기 개발 단계에서는 구현체로 가벼운 메모리 기반의 데이터 저장소 사용
   4. 일반적인 웹 애플리케이션 계층 구조
      1. 컨트롤러 : 웹 MVC의 컨트롤러 역할
      2. 서비스 : 핵심 비즈니스 로직 구현
      3. 리포지토리 : 데이터베이스에 접근, 도메인 객체를 DB에 저장하고 관리
      4. 도메인 : 비즈니스 도메인 객체, 예) 회원, 주문, 쿠폰 등등 주로 데이터베이스에 저장하고 관리됨

2. 회원 도메인과 리포지토리 만들기
   1. 회원 객체 생성
      1. id, name 선언
      2. Getter, Setter 구현
   2. 회원 리포지토리 인터페이스
      1. Member save(Member member);
      2. Optional<Member> findById(Long id);
      3. Optional<Member> findByName(String name);
      4. List<Member> findAll();
      * Optional 객체의 경우 null값이어도 예외가 발생하지 않고 빈 Optional 객체를 반환하게 할 수 있다.(JAVA 8버전부터 Optional이 추가 됨)
   3. 회원 리포지토리 메모리 구현체

3. 회원 리포지토리 테스트 케이스 작성
   1. 개발한 기능을 실행해서 테스트 할 때 자바의 main 메서드를 통해서 실행하거나, 웹 애플리케이션의 컨트롤러를 통해서 해당 기능을 실행한다.<br/>
      이러한 방법은 준비하고 실행하는데 오래 걸리고, 반복 실행하기 어렵고 여러 테스트를 한번에 실행하기 어렵다는 단점이 있다.<br/>
      자바는 JUnit이라는 프레임워크로 테스트를 실행해서 이러한 문제를 해결한다.
   2. src/test/java 하위 폴더에 생성한다.
      1. Assertions.assertThat(result).isEqualTo(member)
         1. Assertions.assertThat을 통해 result와 member의 값이 같은지 비교한다.
         2. 이때 Assertions.assertThat에서 alt + Enter 누르면 Add on-demand static import~ 기능으로 선언할 수 있다.
            1. import static org.assertj.core.api.Assertions.*;
         3. 한번에 여러 테스트를 실행하면 메모리 DB에 직전 테스트의 결과가 남을 수 있다.<br/>
            이렇게 되면 다음 이전 테스트 때문에 다음 테스트가 실패할 가능성이 있다.<br/>
            @AfterEach를 사용하면 각 테스트가 종료될 때 마다 이 기능을 실행한다.<br/>
            여기서는 메모리 DB에 저장된 데이터를 삭제한다.<br/>
            <pre>
            public void clearStore() {
               store.clear();
            }
            
            @AfterEach
            public void afterEach() {
               repository.clearStore();
            }
            </pre>
   3. 테스트는 각각 독립적으로 실행되어야 한다. 테스트 순서에 의존관계가 있는 것은 좋은 테스트가 아니다.

========== 회원 관리 예제 - 백엔드 개발 End ==========
