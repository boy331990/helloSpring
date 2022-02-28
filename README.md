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

4. 회원 서비스 개발
   1. service 패키지, MemberService 생성
   2. MemberRepository 선언
   3. 회원가입, 전체회원 조회 서비스 추

5. 회원 서비스 테스트
   1. 기존에는 회원 서비스가 메모리 회원 리포지토리를 직접 생성하게 했다.
      1. MemoryMemberRepository memberRepository = new MemoryMemberRepository();
      2. 이럴 경우 생성된 메모리 회원 리포지토리가 MemberService의 메모리 회원 리포지토리와 다르다.
         1. 회원 리포지토리의 코드가 회원 서비스 코드를 DI 가능하게 변경한다.
         <pre>
         public class MemberService {
            private final MemberRepository memberRepository;
            
            //생성자 자동생성 단축키: command + n
            public MemberService(MemberRepository memberRepository) {
               this.memberRepository = memberRepository;
            }
         }
         </pre>
         2. MemberServiceTest 클래스에서 @BeforeEach 어노테이션 추가
            1. 각 테스트 실행 전에 호출된다. 테스트가 서로 영향이 없도록 항상 새로운 객체를 생성하고, 의존관계도 새로 맺어준다.

========== 회원 관리 예제 - 백엔드 개발 End ==========

========== 스프링 빈과 의존관계 Start ==========
1. 컴포넌트 스캔과 자동 의존관계 설정
   1. 컨트롤러에 @Controller를 선언
   2. 생성자에 @Autowired 선언
      1. @Autowired가 있으면 스프링이 연관된 객체를 컨테이너에서 찾아서 넣어준다. 이렇게 객체 의존관계를 외부에서 넣어주는 것을 DI(Dependency Injection), 의존성 주입이라 한다.
   3. 컴포넌트 스캔 원리
      1. @Component 애노테이션이 있으면 스프링 빈으로 자동 등록된다.
      2. @Controller 컨트롤러가 스프링 빈으로 자동 등록된 이유도 컴포넌트 스캔 때문이다.
      3. @Component를 포함하는 다음 애노테이션도 스프링 빈으로 자동 등록된다.
         1. @Controller
         2. @Service
         3. @Repository
      4. 생성자에 @Autowired를 사용하면 객체 생성 시점에 스프링 컨테이너에서 해당 스프링 빈을 찾아서 주입한다. 생성자가 1개만 있으면 @Autowired는 생략할 수 있다.
      5. 스프링은 스프링 컨테이너에 스프링 빈을 등록할 때, 기본으로 싱글톤으로 등록한다(유일하게 하나만 등록해서 공유한다) 따라서 같은 스프링 빈이면 모두 같은 인스턴스다. 설정으로 싱글톤이 아니게 설정할 수 있지만, 특별한 경우를 제외하면 대부분 싱글톤을 사용한다.

2. 자바 코드로 직접 스프링 빈 등록하기
   1. 기존 @Service, @Repository, @Autowired 애노테이션을 제거하고 진행
   2. SpringConfig class 파일 생성
      1. @Configuration 애노테이션 추가
      2. MemberService, MemberRepository 메소드 생성 후 @Bean 애노테이션 추가
   3. DI에는 필드 주입, setter 주입, 생성자 주입 이렇게 3가지 방법이 있다. 의존관계가 실행중에 동적으로 변하는 경우는 거의 없으므로 생성자 주입을 권장한다.
      1. 필드 주입
         <pre>
         @Autowired
         private MemberRepository memberRepository;
         </pre>
      2. setter 주입
         <pre>
         private MemberRepository memberRepository;
         
         @Autowired
         public void setMemberRepository(MemberRepository memberRepository) {
            this.memberRepository = memberRepository;
         }
         </pre>
      3. 생성자 주입
         <pre>
         private final MemberRepository memberRepository;
         
         @Autowired
         public MemberService(MemberRepository memberRepository) {
            this.memberRepository = memberRepository;
         }
         </pre>

========== 스프링 빈과 의존관계 End ==========

========== 회원 관리 예제 - 웹 MVC 개발 Start ==========
1. 회원 웹 기능 - 홈 화면 추가
   1. Home 컨트롤러 추가
   2. home.html 추가

2. 회원 웹 기능 - 등록
   1. 웹 등록 화면에서 데이터를 전달 받을 폼 객체
      1. MemberForm class 생성
      2. private String name, getter, setter 생성
   2. 회원 컨트롤러에서 회원을 실제 등록하는 기능
      1. PostMapping(value = "/members/new")으로 받고, create 메소드 생성
         <pre>
         @PostMapping(value = "/members/new")
         public String create(MemberForm form) {
            Member member = new Member();
            member.setName(form.getName());
            memberService.join(member);
            
            return "redirect:/";
         }
         </pre>
   
3. 회원 웹 기능 - 조회
   1. 회원 컨트롤러에서 조회 기능
      1. @GetMapping(value = "/members")으로 받고, list 메소드 생성
      <pre>
      @GetMapping(value = "/members")
      public String list(Model model) {
         List<Member> members = memberService.findMembers();
         model.addAttribute("members", members); 
         return "members/memberList";
      }
      </pre>
   2. 회원 리스트 HTML 추가
   
========== 회원 관리 예제 - 웹 MVC 개발 End ==========

========== 스프링 DB 접근 기술 Start ==========
1. H2 데이터베이스 설치
   1. 개발이나 테스트 용도로 가볍고 편리한 DB, 웹 화면 제공
   2. 설치 가이드
      1. https://www.h2database.com
      2. 다운로드 및 설치(1.4.200 버전)
      3. 권한 주기: chmod 755 h2.sh (윈도우 사용자는 x)
      4. 실행: ./h2.sh (윈도우 사용자는 h2.bat)
      5. 데이터베이스 파일 생성 방법
         jdbc:h2:~/test (최초 한번)
         ~/test.mv.db 파일 생성 확인
         이후부터는 jdbc:h2:tcp://localhost/~/test 이렇게 접속
   3. 테이블 생성하기
      <pre>
      drop table if exists member CASCADE;
      create table member
      (
      id   bigint generated by default as identity,
      name varchar(255),
      primary key (id)
      );
      </pre>
      1. H2 데이터베이스에 접근해서 member 테이블 생성
   * 웹 브라우저가 자동 실행되면 주소창에 임의의 숫자가 표기될때는 localhost로 변경한다.

2. 순수 JDBC
   1. build.gradle 파일에 jdbc, h2 데이터베이스 관련 라이브러리 추가
      1. implementation 'org.springframework.boot:spring-boot-starter-jdbc'
         runtimeOnly 'com.h2database:h2'
   2. 스프링 부트 데이터베이스 연결 설정 추가
      1. resources/application.properties
         1. spring.datasource.url=jdbc:h2:tcp://localhost/~/test
            spring.datasource.driver-class-name=org.h2.Driver
            spring.datasource.username=sa
         * 주의!: 스프링부트 2.4부터는 spring.datasource.username=sa 를 꼭 추가해주어야 한다. 그렇지 않으면 Wrong user name or password 오류가 발생한다. 참고로 다음과 같이 마지막에 공백이 들어가면 같은 오류가 발생한다. spring.datasource.username=sa 공백 주의, 공백은 모두 제거해야 한다.
   3. JDBC 리포지토리 구현
      1. JDBC API로 직접 코딩하는 것은 20년 전 이야기이다. 그냥 넘어가자.
      2. 스프링 설정 변경
         1. SpringConfig파일 내 memberRepository의 return값을 변경
            1. return new MemoryMemberRepository() -> return new JdbcMemberRepository(dataSource);
               1. DataSource는 데이터베이스 커넥션을 획득할 때 사용하는 객체다. 스프링 부트는 데이터베이스 커넥션 정보를 바탕으로 Data Source를 생성하고 스프링 빈으로 만들어둔다. 그래서 DI를 받을 수 있다.

3. 스프링 통합 테스트
   1. @SpringBootTest: 스프링 컨테이너와 테스트를 함께 실행한다.
   2. @Transactional: 테스트 케이스에 이 애노테이션이 있으면, 테스트 시작전에 트랜잭션을 시작하고, 테스트 완료 후에 항상 롤백한다. 이렇게 하면 DB에 데이터가 남지 않으므로 다음 테스트에 영향을 주지 않는다.

4. 스프링 JdbcTemplate
   1. 순수 Jdbc와 동일한 환경설정을 하면된다.
      1. implementation 'org.springframework.boot:spring-boot-starter-jdbc'
   2. 스프링 JdbcTemplate과 MyBatis 같은 라이브러리는 JDBC API에서 본 반복 코드를 대부분 제거해준다. 하지만 SQL은 직접 작성해야 한다.
   
5. JPA
   1. JPA는 기존의 반복 코드는 물론이고, 기본적인 SQL도 JPA가 직접 만들어서 실행해준다.
   2. JPA를 사용하면, SQL과 데이터 중심의 설계에서 객체 중심의 설계로 패러다임을 전환을 할 수 있다.
   3. JPA를 사용하면 개발 생산성을 크게 높일 수 있다.
   4. build.gradle 파일에 JPA, h2 데이터베이스 관련 라이브러리 추가
      1. implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
      2. runtimeOnly 'com.h2database:h2'
   5. 스프링 부트에 JPA 설정 추가(resources/application.properties)
      1. spring.jpa.show-sql=true
         spring.jpa.hibernate.ddl-auto=none
      * show-sql : JPA가 생성하는 SQL을 출력한다.
      * ddl-auto : JPA는 테이블을 자동으로 생성하는 기능을 제공하는데 none 를 사용하면 해당 기능을 끈다.
      * create 를 사용하면 엔티티 정보를 바탕으로 테이블도 직접 생성해준다.
   6. JPA 엔티티 매핑
      1. Member 클래스에 Entity 애노테이션 추가
      2. Id, GeneratedValue(strategy = GenerationType.IDENTITY) 추가
   7. JPA 회원 리포지토리
      1. EntityManager 선언
   8. 서비스 계층에 트랜잭션 추가
      1. @Transactional
         1. org.springframework.transaction.annotation.Transactional
         2. 스프링은 해당 클래스의 메서드를 실행할 때 트랜잭션을 시작하고, 메서드가 정상 종료되면 트랜잭션을 커밋한다. 만약 런타임 예외가 발생하면 롤백한다.
         * JPA를 통한 모든 데이터 변경은 트랜잭션 안에서 실행해야 한다.
   9. JPA를 사용하도록 스프링 설정 변경
      1. private EntityManager em; 
         @Autowired 
         public SpringConfig(EntityManager em) { 
            this.em = em; 
         }
      2. memberRepository() 메소드 내에서 return new JpaMemberRepository(em); 변경

6. 스프링 데이터 JPA
   1. 스프링 데이터 JPA를 사용하면, 기존의 한계를 넘어 마치 마법처럼, 리포지토리에 구현 클래스 없이 인터페이스 만으로 개발을 완료할 수 있습니다. 그리고 반복 개발해온 기본 CRUD 기능도 스프링 데이터 JPA가 모두 제공합니다.
   2. 스프링 데이터 JPA 회원 리포지토리
      <pre>
      public interface SpringDataJpaMemberRepository extends JpaRepository<Member, Long>, MemberRepository {
         Optional<Member> findByName(String name);
      }
      </pre>
   3. 스프링 데이터 JPA 회원 리포지토리를 사용하도록 스프링 설정 변경
      <pre>
      private final MemberRepository memberRepository;
         public SpringConfig(MemberRepository memberRepository) {
         this.memberRepository = memberRepository;
      }

      @Bean
      public MemberService memberService() {
          return new MemberService(memberRepository);
      }
      </pre>
      * 스프링 데이터 JPA가 SpringDataJpaMemberRepository 를 스프링 빈으로 자동 등록해준다.
   4. 스프링 데이터 JPA 제공 클래스
      1. 스프링 데이터
         1. Interface(Repository)
         2. Interface(CrudRepository)
            1. save(S) : S
            2. findOne(ID) : T
            3. exists(ID) : boolean
            4. count() : long
            5. delete(T)
            ...
         3. Interface(PagingAndSortingRepository)
            1. findAll(Sort) : Iterable<T>
            2. findAll(Pageable) : Page<T>
      2. 스프링 데이터 JPA
         1. Interface(JpaRepository)
            1. findAll() : List<T>
            2. findAll(Sort) : List<T>
            3. findAll(Iterable<ID>) : LIST<T>
            4. save(Iterable<ID>) : List<S>
            5. flush()
            6. saveAndFlush(T) : T
            7. deleteInBatch(Iterable<T>)
            8. deleteAllInBatch()
            9. getOne(ID) : T
      3. 스프링 데이터 JPA 제공 기능
         1. 인터페이스를 통한 기본적인 CRUD
         2. findByName(), findByEmail() 처럼 메서드 이름만으로 조회 기능 제공
         3. 페이징 기능 자동 제공
      * 실무에서는 JPA와 스프링 데이터 JPA를 기본으로 사용하고, 복잡한 동적 쿼리는 Querydsl이라는 라이브러리를 사용하면 된다. Querydsl을 사용하면 쿼리도 자바 코드로 안전하게 작성할 수 있고, 동적 쿼리도 편리하게 작성할 수 있다. 이 조합으로 해결하기 어려운 쿼리는 JPA가 제공하는 네이티브 쿼리를 사용하거나, 앞서 학습한 스프링 JdbcTemplate를 사용하면 된다.
   
========== 스프링 DB 접근 기술 End ==========

========== AOP Start ==========
1. AOP가 필요한 상황
   1. 모든 메소드의 호출 시간을 측정하고 싶다면?
   2. 공통 관심 사항 vs 핵심 관심 사항
   3. 회원 가입 시간, 회원 조회 시간을 측정하고 싶다면?

2. AOP 적용
   1. AOP : Aspect Oriented Programming
========== AOP End ==========