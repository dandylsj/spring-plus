# SPRING PLUS
Spring Boot 기반 할 일 관리 시스템

JWT 인증, JPA 최적화, QueryDSL, AOP 등 Spring Boot 실무 핵심 개념을 적용한 Todo 관리 애플리케이션입니다.

📋 목차
주요 기능
기술 스택
프로젝트 구조
시작하기
API 문서
핵심 구현 사항
ERD
✨ 주요 기능
인증 및 사용자 관리
JWT 기반 회원가입 및 로그인
BCrypt 비밀번호 암호화
사용자 권한 관리 (USER, ADMIN)
관리자 전용 사용자 권한 변경 기능
할 일 관리
할 일 생성, 조회, 수정, 삭제 (CRUD)
외부 날씨 API 연동으로 생성 시점 날씨 정보 저장
날씨별, 기간별 할 일 검색 및 필터링
페이징 처리된 할 일 목록 조회
댓글 기능
할 일에 대한 댓글 작성 및 조회
작성자 정보 포함한 댓글 조회
담당자 관리
할 일에 담당자 추가/삭제
할 일 생성 시 작성자 자동 담당자 등록
담당자 목록 조회
🛠 기술 스택
Backend
Java 17
Spring Boot 3.3.3
Spring Web
Spring Data JPA
Spring Security
Spring Boot Validation
QueryDSL 5.0.0 - 타입 안전 쿼리
Database
MySQL (Production)
H2 (Test)
Hibernate - ORM
Security
JWT (JJWT 0.11.5) - 토큰 기반 인증
BCrypt 0.10.2 - 비밀번호 암호화
Build Tool
Gradle 8.8
Other
Lombok - 보일러플레이트 코드 감소
📁 프로젝트 구조
src/main/java/org/example/expert/
├── ExpertApplication.java
├── aop/
│   └── AdminAccessLoggingAspect.java      # 관리자 접근 로깅 AOP
├── client/
│   ├── WeatherClient.java                  # 외부 날씨 API 클라이언트
│   └── dto/WeatherDto.java
├── config/
│   ├── AuthUserArgumentResolver.java       # @Auth 파라미터 리졸버
│   ├── FilterConfig.java                   # Filter 설정
│   ├── GlobalExceptionHandler.java         # 전역 예외 처리
│   ├── JwtFilter.java                      # JWT 인증 필터
│   ├── JwtUtil.java                        # JWT 토큰 유틸리티
│   ├── PasswordEncoder.java                # 비밀번호 인코더
│   ├── PersistenceConfig.java              # JPA Auditing 설정
│   ├── QueryDslConfig.java                 # QueryDSL 설정
│   └── WebConfig.java                      # Web MVC 설정
└── domain/
    ├── common/                              # 공통 컴포넌트
    │   ├── annotation/Auth.java
    │   ├── dto/AuthUser.java
    │   ├── entity/Timestamped.java
    │   └── exception/
    ├── auth/                                # 인증 도메인
    │   ├── controller/
    │   ├── dto/
    │   ├── service/
    │   └── exception/
    ├── user/                                # 사용자 도메인
    │   ├── controller/
    │   ├── dto/
    │   ├── entity/User.java
    │   ├── enums/UserRole.java
    │   ├── repository/
    │   └── service/
    ├── todo/                                # 할 일 도메인
    │   ├── controller/
    │   ├── dto/
    │   ├── entity/Todo.java
    │   ├── repository/
    │   │   ├── TodoRepository.java
    │   │   ├── TodoRepositoryCustom.java
    │   │   └── TodoRepositoryImpl.java     # QueryDSL 구현
    │   └── service/
    ├── comment/                             # 댓글 도메인
    │   ├── controller/
    │   ├── dto/
    │   ├── entity/Comment.java
    │   ├── repository/
    │   └── service/
    └── manager/                             # 담당자 도메인
        ├── controller/
        ├── dto/
        ├── entity/Manager.java
        ├── repository/
        └── service/
🚀 시작하기
사전 요구사항
Java 17 이상
MySQL 8.0 이상
Gradle 8.8 이상
데이터베이스 설정
MySQL에 데이터베이스를 생성합니다:

CREATE DATABASE expert;
환경 변수 설정
src/main/resources/application.yaml 파일을 확인하고 필요시 수정합니다:

spring:
  datasource:
    url: jdbc:mysql://localhost:3306/expert
    username: your_username
    password: your_password

jwt:
  secret:
    key: your_secret_key_here
실행 방법
프로젝트 클론
git clone <repository-url>
cd spring-plus
빌드 및 실행
./gradlew build
./gradlew bootRun
애플리케이션이 http://localhost:8080에서 실행됩니다.
📚 API 문서
인증 API
회원가입
POST /auth/signup
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "password123",
  "userRole": "USER",
  "nickName": "홍길동"
}
로그인
POST /auth/signin
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "password123"
}
응답:

{
  "bearerToken": "Bearer eyJhbGciOiJIUzI1NiJ9..."
}
할 일 API
할 일 생성
POST /todos
Authorization: Bearer {token}
Content-Type: application/json

{
  "title": "프로젝트 완성하기",
  "contents": "Spring Boot 프로젝트를 완성한다"
}
할 일 목록 조회
GET /todos?page=1&size=10&weather=맑음&startDate=2025-01-01T00:00:00&endDate=2025-12-31T23:59:59
할 일 상세 조회
GET /todos/{todoId}
댓글 API
댓글 작성
POST /todos/{todoId}/comments
Authorization: Bearer {token}
Content-Type: application/json

{
  "contents": "댓글 내용입니다"
}
댓글 목록 조회
GET /todos/{todoId}/comments
담당자 API
담당자 추가
POST /todos/{todoId}/managers
Authorization: Bearer {token}
Content-Type: application/json

{
  "managerUserId": 2
}
담당자 목록 조회
GET /todos/{todoId}/managers
담당자 삭제
DELETE /todos/{todoId}/managers/{managerId}
Authorization: Bearer {token}
관리자 API
사용자 권한 변경 (ADMIN 전용)
PATCH /admin/users/{userId}
Authorization: Bearer {token}
Content-Type: application/json

{
  "userRole": "ADMIN"
}
🔧 핵심 구현 사항
1. JWT 기반 인증 시스템
JwtFilter를 통한 요청 인증
모든 요청에 대해 JWT 토큰 검증
/auth/** 경로는 인증 제외
/admin/** 경로는 ADMIN 권한 추가 검증
토큰에서 추출한 사용자 정보를 HttpServletRequest에 저장
ArgumentResolver를 통한 인증 정보 주입
@GetMapping("/todos")
public ResponseEntity<Page<TodoResponse>> getTodos(@Auth AuthUser authUser) {
    // authUser에 인증된 사용자 정보가 자동으로 주입됨
}
2. N+1 문제 완벽 해결
QueryDSL을 활용한 Fetch Join
@Override
public Optional<Todo> findByIdWithUser(Long todoId) {
    return Optional.ofNullable(
        queryFactory
            .selectFrom(todo)
            .leftJoin(todo.user, user).fetchJoin()  // N+1 해결
            .where(todo.id.eq(todoId))
            .fetchOne()
    );
}
JPQL JOIN FETCH 사용
@Query("SELECT c FROM Comment c JOIN FETCH c.user WHERE c.todo.id = :todoId")
List<Comment> findByTodoIdWithUser(@Param("todoId") Long todoId);

@Query("SELECT m FROM Manager m JOIN FETCH m.user WHERE m.todo.id = :todoId")
List<Manager> findByTodoIdWithUser(@Param("todoId") Long todoId);
3. JPA Cascade를 활용한 연관 관계 관리
할 일 생성 시 작성자 자동 담당자 등록
@Entity
public class Todo extends Timestamped {
    @OneToMany(mappedBy = "todo", cascade = CascadeType.PERSIST)
    private List<Manager> managers = new ArrayList<>();

    public Todo(String title, String contents, String weather, User user) {
        this.title = title;
        this.contents = contents;
        this.weather = weather;
        this.user = user;
        this.managers.add(new Manager(user, this));  // 자동 담당자 등록
    }
}
할 일 삭제 시 댓글 자동 삭제
@OneToMany(mappedBy = "todo", cascade = CascadeType.REMOVE)
private List<Comment> comments = new ArrayList<>();
4. AOP를 활용한 관리자 접근 로깅
@Aspect
@Component
public class AdminAccessLoggingAspect {
    @Before("execution(* org.example.expert.domain.user.controller.UserAdminController.changeUserRole(..))")
    public void logBeforeChangeUserRole(JoinPoint joinPoint) {
        log.info("Admin Access Log - User ID: {}, Request Time: {}, Request URL: {}, Method: {}",
                userId, requestTime, requestUrl, joinPoint.getSignature().getName());
    }
}
5. 트랜잭션 최적화
읽기 전용 트랜잭션 분리
@Service
@RequiredArgsConstructor
public class TodoService {
    @Transactional  // 쓰기 작업
    public TodoSaveResponse saveTodo(AuthUser authUser, TodoSaveRequest todoSaveRequest) {
        // ...
    }

    @Transactional(readOnly = true)  // 읽기 작업 최적화
    public Page<TodoResponse> getTodos(...) {
        // ...
    }
}
6. 날씨 API 연동
할 일 생성 시 외부 날씨 API를 호출하여 현재 날씨 정보를 저장합니다:

public TodoSaveResponse saveTodo(AuthUser authUser, TodoSaveRequest todoSaveRequest) {
    User user = User.fromAuthUser(authUser);

    // 외부 날씨 API 호출
    String weather = weatherClient.getTodayWeather();

    Todo newTodo = new Todo(
        todoSaveRequest.getTitle(),
        todoSaveRequest.getContents(),
        weather,  // 날씨 정보 저장
        user
    );
    // ...
}
7. 다양한 검색 및 필터링
날씨별 검색: 특정 날씨의 할 일만 조회
기간별 검색: 시작일~종료일 사이의 할 일 조회
페이징 처리: Spring Data JPA Pageable 활용
정렬: 수정일 기준 내림차순
📊 ERD
┌─────────────┐         ┌─────────────┐         ┌─────────────┐
│    User     │         │    Todo     │         │   Comment   │
├─────────────┤         ├─────────────┤         ├─────────────┤
│ id (PK)     │───┐     │ id (PK)     │────┬───>│ id (PK)     │
│ email       │   │     │ title       │    │    │ contents    │
│ password    │   │     │ contents    │    │    │ user_id(FK) │
│ userRole    │   │     │ weather     │    │    │ todo_id(FK) │
│ nickName    │   │     │ user_id(FK) │<───┘    │ createdAt   │
│ createdAt   │   │     │ createdAt   │         │ modifiedAt  │
│ modifiedAt  │   │     │ modifiedAt  │         └─────────────┘
└─────────────┘   │     └─────────────┘
                  │            │
                  │            │
                  │            ├────>┌─────────────┐
                  │            │     │   Manager   │
                  │            │     ├─────────────┤
                  │            │     │ id (PK)     │
                  └────────────┼────>│ user_id(FK) │
                               │     │ todo_id(FK) │
                               │     └─────────────┘
                               │
                               └─────── CascadeType.PERSIST (Todo → Manager)
                                       CascadeType.REMOVE (Todo → Comment)
엔티티 관계
User - Todo: 1:N (한 사용자는 여러 할 일 작성)
User - Comment: 1:N (한 사용자는 여러 댓글 작성)
Todo - Comment: 1:N (한 할 일에 여러 댓글, Cascade REMOVE)
User - Manager: 1:N (한 사용자는 여러 할 일의 담당자)
Todo - Manager: 1:N (한 할 일에 여러 담당자, Cascade PERSIST)
🎯 주요 개선 이력
1. QueryDSL 도입
타입 안전한 쿼리 작성
Fetch Join을 통한 N+1 문제 해결
동적 쿼리 작성 용이
2. N+1 문제 해결
Comment 조회 시 User 정보 JOIN FETCH
Manager 조회 시 User 정보 JOIN FETCH
Todo 조회 시 QueryDSL fetchJoin 사용
3. JWT에 닉네임 추가
토큰 Claims에 nickName 추가
API 응답에 닉네임 포함
null 문제 해결
4. Cascade 설정 최적화
Todo 생성 시 Manager 자동 저장 (PERSIST)
Todo 삭제 시 Comment 자동 삭제 (REMOVE)
연관 관계 관리 자동화
5. AOP 개선
@After → @Before로 변경
메서드 실행 전 로깅으로 실패 케이스도 추적 가능
6. 트랜잭션 최적화
읽기 전용 트랜잭션 분리 (readOnly=true)
Dirty Checking 방지로 성능 향상
7. 검색 기능 강화
날씨별 필터링
날짜 범위 검색
다양한 조회 조건 지원
📝 라이선스
이 프로젝트는 학습 목적으로 제작되었습니다.

👤 Author
sejinmac
