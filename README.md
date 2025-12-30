# SPRING PLUS
Spring Boot 기반 할 일 관리 시스템

JWT 인증, JPA 최적화, QueryDSL, AOP 등 Spring Boot 실무 핵심 개념을 적용한 Todo 관리 애플리케이션입니다.

# 🛠 기술 스택
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
Gradle 8.8


## 🎯 주요 개선 이력
### 1. QueryDSL 도입
타입 안전한 쿼리 작성
Fetch Join을 통한 N+1 문제 해결
동적 쿼리 작성 용이
### 2. N+1 문제 해결
Comment 조회 시 User 정보 JOIN FETCH
Manager 조회 시 User 정보 JOIN FETCH
Todo 조회 시 QueryDSL fetchJoin 사용
### 3. JWT에 닉네임 추가
토큰 Claims에 nickName 추가
API 응답에 닉네임 포함
null 문제 해결
### 4. Cascade 설정 최적화
Todo 생성 시 Manager 자동 저장 (PERSIST)
Todo 삭제 시 Comment 자동 삭제 (REMOVE)
연관 관계 관리 자동화
### 5. AOP 개선
@After → @Before로 변경
메서드 실행 전 로깅으로 실패 케이스도 추적 가능
### 6. 트랜잭션 최적화
읽기 전용 트랜잭션 분리 (readOnly=true)
Dirty Checking 방지로 성능 향상
### 7. 검색 기능 강화
날씨별 필터링
날짜 범위 검색
다양한 조회 조건 지원


