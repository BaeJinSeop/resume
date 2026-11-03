# 배진섭

**백엔드 개발자 | 경력 9년 | Java, Kotlin, Spring Boot**

[poxy1535@gmail.com](mailto:poxy1535@gmail.com) | [GitHub](https://github.com/BaeJinSeop) | [상세 포트폴리오](./PORTFOLIO.md)

## 소개

2017년부터 웹 서비스와 앱을 만들어 왔고, 2022년부터 Java와 Kotlin, Spring Boot로 사내 서버 15개 이상을 개발하거나 유지보수했습니다. 지금은 동물병원 클라우드 차트 백엔드에서 초기 설계와 대부분의 도메인을 개발하고, AWS 인프라와 배포, DB 마이그레이션을 혼자 구축해 운영합니다. 통합 로그인, 메시지 발송, 병원 데이터 수집 같은 Java 레거시 서버도 넘겨받아 운영해 왔습니다. 병원마다 스키마가 나뉜 환경에서 스키마 변경, DB 복제, 무중단 배포가 부딪히는 문제를 주로 풀어 왔습니다.

## 기술

- 백엔드: Java 8\~21, Kotlin, Spring Boot 2\~4, Spring MVC, WebFlux, Spring Security, Spring Batch, 코루틴, R2DBC, 가상 스레드, JPA/QueryDSL, MyBatis, JDBC
- 데이터: Aurora MySQL, MySQL, MariaDB, Redis/Valkey, MongoDB, Flyway, 멀티테넌트 스키마
- 인프라: AWS(EC2, ASG, ALB, CodeDeploy, S3, ECR, DMS, Secrets Manager), GitHub Actions, Docker, Linux, systemd, nginx, Tomcat
- 모니터링과 메시징: Prometheus, Grafana, Loki, Tempo, Alertmanager, Kafka, STOMP/WebSocket
- 테스트: JUnit 5, MockK, Testcontainers
- 그 밖에: Android 네이티브(Java), PHP/CodeIgniter, TypeScript, Vue, React, React Native, Flutter, SwiftUI

## 경력

### (주)인투씨엔에스

**2021.10 ~ 현재** | 개발1팀 책임연구원

동물병원 IT 솔루션 회사. 클라우드 차트 백엔드, 통합 로그인, 메시지 발송과 과금, 병원 데이터 수집과 백업, 키오스크 등 현장 단말 담당

#### 클라우드 EMR 백엔드

**2024.05 ~ 현재** | Kotlin, Spring WebFlux, R2DBC, Aurora MySQL, AWS

50개 이상 병원이 병원별 DB 스키마로 쓰는 API 서버. 자사 온프레미스 차트 이관, 타사 차트 전환, 신규 도입으로 병원 수 계속 증가 중. 백엔드 2명 중 설계와 주요 도메인 담당, 인프라, 배포, 마이그레이션은 단독 구축

- Kotlin, WebFlux, R2DBC로 백엔드 구성. 진료계획, 처방, 수납, 차트, 예약, 재고, 권한 도메인 개발. 이후 16개 도메인을 기능별 수직 슬라이스 구조로 재편
- GitHub Actions와 CodeDeploy로 Blue/Green 배포 구축, 누적 250회 이상 릴리스. 스키마 변경 중 DMS 복제가 깨지지 않도록 배포 단계에 복제 중지와 재개를 연결
- Flyway로 병원별 스키마 마이그레이션 자동화(마이그레이션 파일 230개). 수집 DB에는 DDL만 적용하고 DML은 DMS 복제에 맡겨 중복 반영 방지. 커넥션 동적 갱신으로 재배포 없이 병원 추가
- 신규 병원 개설 절차(복제 중지, 주 DB와 수집 DB 스키마 생성, 복제 재개, 초기 설정)를 프로비저닝 API와 관리자 화면 일괄 실행으로 자동화. 실행 기록 행 잠금으로 중복 실행을 막고, 연결이 끊겨도 복제 재개 보장
- 외부 DB가 잠깐 끊기면 ASG가 정상 인스턴스까지 교체하던 장애를 liveness와 readiness 분리로 해결
- 웹, 모바일, 소켓 서버 간 타임라인 이벤트 계약 조율과 발행, 중계 경로 구현, 백필 멱등 처리

#### 파일 업로드 서버 분리

**2026.09 ~ 2026.10** | Kotlin, Spring WebFlux, R2DBC, AWS

- 이미지와 영상 업로드를 진료 API 서버에서 독립 서버로 단독 분리. 임시 파일 기반 S3 전송, 동시 처리 수를 제한한 이미지 변환, 기존 계약과 인증 유지로 API 주소 교체만으로 전환 가능하게 구성
- ARM 골든 AMI, ALB, ASG, CodeDeploy Blue/Green 운영 환경과 OIDC 기반 태그 배포 파이프라인 단독 구축

#### 백업 파일 서버

**2026.05 ~ 2026.06** | Java 21, Spring Boot, 가상 스레드, JDBC, MariaDB

- 1,000여 병원의 백업을 받는 온프레미스 서버 단독 설계와 구축. TUS로 최대 50GB 단일 파일 이어 올리기, SHA-256 검증과 일일 재검증, 병원별 쿼터
- Java 21 가상 스레드와 JDBC로 구현. 유지보수가 끊긴 업로드 라이브러리는 포트 뒤로 분리. 동시 업로드 쿼터 경합 수정, 업로드 E2E를 포함한 통합 테스트 작성

#### 메시지 발송과 과금 서버

**2023.02 ~ 현재** | Java 8, Kotlin, Spring Boot, Spring Batch, MyBatis, MySQL

- Java 8 SMS와 알림톡 서버를 넘겨받아 2024.11까지 단독 유지보수, 이후 동료와 분담. 병원별 포인트 차감 API와 이력 조회, 월별 발송 로그 기준 중복 발송 차단과 멱등 응답, 080 수신거부 증분 동기화 개발
- 앱 푸시를 먼저 보내고 미가입자와 발송 실패자만 문자로 대체하는 통합 발송 API 개발. 예약 발송은 Spring Batch 청크 처리로 구현하고 포인트 확인, 수신거부 제외, 중복 과금 방지 적용
- 2026.09 클라우드용 API 5개를 Kotlin으로 단독 재작성. 기존 DB와 발송 엔진 계약 유지, 멱등성 테이블과 유니크 제약으로 중복 차감과 환불 차단, nginx 롤링 배포

#### 통합 로그인 서버

**2024.09 ~ 현재** | Java 8, Spring Boot, Spring Security, JPA/QueryDSL, Redis

- 사내 웹 서비스 5개가 함께 쓰는 통합 로그인 서버를 넘겨받아 2024.09부터 단독 유지보수와 기능 개발. JWT 토큰과 Redis 세션, 7개 DB를 묶은 분산 트랜잭션(JTA) 구조 운영
- 회원, 본인인증 기록, 병원 연락처 등 4개 DB, 7개 테이블의 개인정보 컬럼 14개 암호화. JPA 엔티티에서 암복호화하고 QueryDSL 프로젝션과 JDBC 조회 경로를 따로 보완해 연동 서비스 응답 형식 유지
- 클라우드 차트에서 병원 포털로 재로그인 없이 넘어가는 연동 로그인 API를 기존 Spring Security 인증 흐름 위에 구현. 가입 승인 알림은 직접 만든 Teams 알림 서버(Java 21)로 이전

#### 병원 데이터 수집과 통계

**2022.05 ~ 2026.06** | Java 8\~17, Spring Boot, MyBatis, Spring Batch, MySQL

- 병원 로컬 DB 변경분을 1분 주기로 중앙 DB에 반영하는 수집 서버를 2022.10부터 단독 유지보수. 서버 2대 작업 분할과 서버별 실행 잠금, 인코딩 혼재 처리, 장애 알림 추가
- 신규 마스터 DB 도입에 맞춰 업로드 파일 이중 적재와 반영 스케줄러로 병행 운영 구성. 병원 백업을 FTP에서 SFTP로 옮기며 격리 계정 프로비저닝, 용량 기반 서버 배정, 라이선스 검증 API 개발
- 본부 통계 배치(통계 워커 24종)를 2022.06부터 단독 유지보수하며 시간대별 고객 수와 매출 집계 추가. 분산 수집 DB 10대 조회 라우팅 구현. 클라우드 고객등급 배치는 병원별 Job으로 실패 격리

#### 운영 관리자 시스템과 사내 인트라넷

**2026.06 ~ 현재** | Kotlin, Spring Boot 3\~4, Spring Security, JPA, Vue 3

- 백엔드, 프론트엔드, 배포 단독 개발. DB 메타데이터에서 빠진 썸네일까지 과금되도록 S3 객체를 직접 집계, 사용량 이력과 접근 제어, 모니터링, 신규 병원 일괄 생성 화면 구현
- PHP 5.6 사내 인트라넷을 Kotlin/Spring Boot 4와 Vue 3로 단독 재작성(운영 전환 준비 중). 레거시 DB를 공유하는 병행 운영 구조, 외부 접속 2차 인증, 요청 단위 권한 재조회, CI 구성

#### 모바일 차트와 실시간 메시징

**2025.01 ~ 현재** | Java 21, Spring Boot, STOMP, Kafka, MongoDB, JPA/QueryDSL

- 기존 소켓 서버에 모바일 채팅, PC와 모바일 간 읽음과 입퇴장 동기화, 서버 간 push 경로 추가. Kafka 컨슈머의 push 판정을 6단계로 나누고 경계 조건을 단위 테스트로 고정
- 모바일 차트 API의 접수현황, 바이탈, 사진 기능과 PC 실시간 연동 개발. 커밋 뒤 발행을 분리하고, 비관적 잠금과 동시성 테스트로 삭제한 바이탈이 되살아나는 문제 차단
- 키오스크 Java API의 인증과 소켓 서버 연동, 마약류 통합관리시스템 투약보고 연동 분담

#### 그 밖의 업무

- 무인 접수와 결제 키오스크 4년 이상 개발과 운영(결제와 주변기기 Android 네이티브 모듈은 Java로 작성, 단말 100대 이상 OTA 관리). 대기실 사이니지와 외부 펫케어 앱 예약 API의 백엔드 단독 담당

### (주)미툰앤노벨

**2018.08 ~ 2021.07** | 개발팀 주임, 풀스택 개발

- 누적 회원 100만 명이 넘는 PHP 콘텐츠 서비스 '미소설' 개발과 운영. 정기 구독 결제, 카카오 알림톡과 FCM 푸시 서버 구축. IDC 다중 웹서버, nginx 로드밸런싱, 다중 DB 복제 환경
- 웹소설 플랫폼 '미노벨'의 작가 정산, PG 결제, 포인트 후원, SNS 통합 로그인 개발

### (주)다누온

**2017.02 ~ 2018.04** | 개발팀 주임, 클라이언트와 서버 개발

- 시각장애인용 리듬 액션 게임 'Full Metal Runner' Unity 클라이언트와 서버 개발, Android와 iOS 출시
- 시각장애인 전용 앱 스토어 'BF Apps'를 CodeIgniter로 구축, 모바일 접근성(MA) 인증 획득 주도

## 개인 프로젝트

#### 짠테크 가계부

**2026.03 ~ 현재** | Flutter, Kotlin, Spring WebFlux, R2DBC

앱과 서버를 혼자 개발해 양대 스토어 출시(3개 언어), 4개월간 14개 버전 배포. 앱 심사 지연을 감안한 서버 선배포와 하위호환 규칙, 오프라인 요청 큐와 서버 멱등키, 사용자 타임존 기반 스케줄링

[App Store](https://apps.apple.com/app/id6778849270) | [Google Play](https://play.google.com/store/apps/details?id=com.snowball.nundeongyi)

#### 기획팀 AI 어시스턴트

**2026.08 ~ 2026.09**

Claude Code, Codex 헤드리스와 MCP로 회의록 작업 추출, 디자인 명세 생성 도구를 만들어 회사 기획팀에 도입. 엔진 공통 MCP 프록시 권한 가드, 쓰기 승인, 감사 로그, 골든 셋 회귀 평가 구현

## 학력

- 학점은행제 컴퓨터공학 학사 | 2021.10 ~ 2026.02 졸업 | 학점 3.88 / 4.5

## 자격증

- 정보처리산업기사 | 2023.11
- 네트워크관리사 2급 | 2023.12
