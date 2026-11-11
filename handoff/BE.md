# 한마음 BE 개발 채팅 인수인계

작업 레포: C:/dev/Hanmaum/hanmaum-backend
먼저 [공통 기준](COMMON.md), [충돌과 미정](DECISIONS.md), [기능 목록](FEATURES.md)을 읽고 관련 제품 정책 장을 확인한다.
사용자 요청 방향은 ERD Cloud에서 ERD 설계 → API 명세/공통 응답 정리 → 구현이다. 아래 테이블 후보·진행 순서는 설계 제안이며 확정 ERD가 아니다.

## 현재 구현

관측 커밋 be7f131, dev, 2026-10-01 조회 당시 작업 트리 깨끗함.
Java 21 / Spring Boot 4.1.1 / Gradle Wrapper 9.7.1 / Spring MVC·JPA / PostgreSQL 17.11 / Flyway.
Spring Security OAuth2 Client, Spring Session JDBC, Validation, Springdoc, Actuator, Spotless, Testcontainers 기반.

| 존재하는 부분 | 파일 | 실제 범위 |
| --- | --- | --- |
| 세션·CSRF·OAuth 기초 | [SecurityConfig](../../hanmaum-backend/src/main/java/com/hanmaum/backend/global/security/SecurityConfig.java) | 인증 필터·로그인 시작/콜백 설정·로그아웃 |
| CSRF 조회 | [CsrfController](../../hanmaum-backend/src/main/java/com/hanmaum/backend/auth/controller/CsrfController.java) | GET /api/auth/csrf |
| 공통 오류 | [ApiError](../../hanmaum-backend/src/main/java/com/hanmaum/backend/global/response/ApiError.java), [예외 처리](../../hanmaum-backend/src/main/java/com/hanmaum/backend/global/exception/GlobalExceptionHandler.java) | code/message/fieldErrors |
| 내부 AI 클라이언트 | [HanmaumAiClient](../../hanmaum-backend/src/main/java/com/hanmaum/backend/ai/client/HanmaumAiClient.java) | POST /v1/analyses, 결과 계약 검증 |
| AI DTO | [AiAnalysisRequest](../../hanmaum-backend/src/main/java/com/hanmaum/backend/ai/dto/AiAnalysisRequest.java), [응답](../../hanmaum-backend/src/main/java/com/hanmaum/backend/ai/dto/AiAnalysisResponse.java) | 후보·버전·근거·제안 계약 |
| DB 초기 마이그레이션 | [V1](../../hanmaum-backend/src/main/resources/db/migration/V1__create_spring_session_tables.sql) | Spring Session 테이블만 |
| 테스트 | src/test | 보안·애플리케이션·AI HTTP 계약 테스트 코드 |

**아직 없는 부분:** 한마음 회원 엔티티/소셜 계정 매핑, 공간·참여·초대·기록·제안/요청·항목·근거/이력 도메인, 최종 승인 트랜잭션, 완성된 공개 API 명세·제품 ERD.
OAuth 기반이 있다는 사실은 실제 사용자 생성·공간 권한·제공자 로그인 검증 완료가 아니다.
정확한 현재 구현 여부는 해당 파일을 다시 확인한다. 이번 문서화에서는 테스트를 실행하지 않았다.

## 첫 번째로 정리할 충돌

[docs/architecture.md](../../hanmaum-backend/docs/architecture.md)의 권한 표는 MEMBER도 항목 등록과 AI 변경을 직접 할 수 있는 것처럼 적혀 있다.
제품 정책 1.3·6장, SPACE-08, PROP-10이 더 구체적이다. **구성원은 일반 변경을 요청하고 소유자가 최종 반영**한다.
기존 문서가 미정이라고 적은 소유권 이전·탈퇴·소식 원본 삭제에도 현재 정책이 존재한다. 관련 표를 최신 제품 기준에 맞춘 뒤 구현한다.
이 인수인계 작업에서는 기존 문서/코드를 직접 고치지 않았다.

## ERD에서 다룰 개념

다음은 논리 설계 체크리스트이며 테이블명·관계·물리 구조를 확정한 목록이 아니다.

- 계정, provider/providerUserId별 소셜 식별, 세션.
- 부모님 한 명을 나타내는 돌봄 공간, 현재/종료 참여, 공간별 OWNER/MEMBER.
- 24시간 초대 링크와 발급자, 중복 참여 방지.
- 원본 소식·작성자·고정된 recordedAt, 분석 실행/처리 상태.
- AI 제안과 구성원 변경 요청, 검토 상태·요청자·반영자·대상 버전.
- 관리 항목 5유형과 유형별 값, 일정/할 일/질문 상태, 단일 담당자.
- 여러 근거와 항목의 연결, 원문 인용의 출처 추적, 직접 입력/답변 근거.
- 변경 전후 이력·행위자·시각. 원본 삭제 시 민감 내용 제거 가능한 구조.
- 항목 편집 잠금, optimistic version 또는 동등한 최신성 검사.
- 반복 요청의 처리 결과/멱등성, 공유 링크와 접근 시점 검증.
- Should 알림을 추가한다면 수신자·읽음·개인 설정·중복 방지.

모델링 검증:
1. 공간마다 소유자 정확히 1명, 이전 중 공백/중복 없음.
2. 권한은 계정 전역이 아닌 공간 참여 관계.
3. 일반 삭제와 원본 내용 제거의 저장 정책 분리.
4. 원본 삭제 대상이 어떤 제안/이력/항목 값으로 전파됐는지 추적 가능.
5. 떠난 사용자는 접근 불가, 미완료 담당 업무 해제, 완료/이력 유지.
6. 분석 중 원본 삭제 시 결과를 안전하게 버릴 수 있음.
7. 선택 묶음 승인 시 전부 성공 또는 전부 미반영.
8. 버전·근거·잠금 충돌을 UI가 이해할 수 있게 응답 가능.

ERD Cloud 파일/링크는 이 채팅에 제공되지 않았다. 담당 채팅에서 접근 가능 여부와 대상 프로젝트를 확인하고 실제 ERD 작성 결과를 전달한다. 도구 접근이 없으면 논리 ERD와 DDL 초안을 먼저 검토 가능하게 만들고, ERD Cloud에 업로드했다고 말하지 않는다.
Flyway V1은 이미 적용 가능한 초기 파일이므로 수정하지 않고 이후 버전 마이그레이션으로 추가한다.

## API 명세 작성 기준

각 API에 기능 ID·진입 화면·권한·입출력·필드 검증·상태 전환·정상/예외 응답·멱등성·트랜잭션·삭제 영향을 적는다.
Figma의 화면 356개를 같은 수의 API로 만들지 않는다. 동일 항목/요청 자원을 여러 화면에서 재사용한다.

우선 정의할 묶음:
1. 현재 사용자/세션·OAuth 후 계정 생성, 공간 목록·생성·진입, 초대 발급/확인/참여.
2. 원본 저장·목록·상세·분석 상태/재시도.
3. 제안·변경 요청 목록/상세·수정·선택 반영·미적용·결과 조회.
4. 항목 CRUD, 단일 담당자, 할 일/일정 상태, 질문 답변/해소.
5. 근거·이력·원본 삭제 정리, 현황 요약, 최소 카드 공유.
6. 가족·소유권·나가기·공간 삭제·탈퇴.

현재 영문 상태명은 일부만 코드에 있다. 공개 상태 enum·ID 종류·페이지네이션·정렬·날짜/시간 표현·오류 코드를 FE와 합의한 뒤 명세에 고정한다.
필드 길이·필수 입력·잠금 TTL 등 미정값을 ERD 작성 중 임의 제품 규칙으로 확정하지 않는다.

## 공통 응답은 기존 구현부터 맞춘다

기존 ApiError는 code/message/fieldErrors이며 인증 오류도 이를 사용한다.
현재 로그아웃 성공은 204, CSRF는 Spring의 토큰 정보 응답, AI 내부 응답은 requestId/suggestions다.
통일된 공개 성공 envelope는 아직 없다. 모든 경로를 기계적으로 data로 감싸거나 204를 JSON 응답으로 바꾸지 않는다.
새 응답 규격이 필요하면 적용 대상·예외·FE 영향과 함께 설계한다.

BE/AI 오류는 현재 외부에 502 AI_SERVICE_UNAVAILABLE로 정리한다. 사용자용 분석 상태에 실패 원인별 재시도를 제공할지와 내부 코드 매핑은 추가 설계가 필요하다. 원문·내부 키·제공자 오류 본문을 사용자나 로그에 노출하지 않는다.

## 인증과 FE 연결

세션 쿠키 HANMAUM_SESSION, HttpOnly/Secure/SameSite=Lax 기반이다. local 프로필 예외 설정을 읽고 운영 설정과 혼동하지 않는다.
현재 session.timeout=12h는 **구현 설정값**이며 제품 정책서에서 확정한 로그인 유지시간이라는 뜻은 아니다.
CSRF는 유지한다. FE credentials: include, 쓰기 요청 CSRF 헤더, 로그인/로그아웃 후 재발급을 맞춘다.
JWT 기반 재설계는 이 인수인계의 요청이 아니다.
이메일이 같다는 이유로 서로 다른 소셜 계정을 자동 병합하지 않는 현재 기초 방향을 유지한다. 계정 연결 기능은 별도 미정이다.

## AI와 함께 유지할 계약

[BE 계약](../../hanmaum-backend/docs/ai-contract.md), [AI 계약](../../hanmaum-ai/docs/ai-contract.md), 양쪽 DTO와 합성 fixture를 같이 본다.
브라우저는 AI 직접 호출을 하지 않는다. Spring이 현재 참여·공간을 검증하고 같은 공간의 유효 후보만 전달한다.
LLM 호출 중 DB 트랜잭션을 열어 두지 않는다. 반환 후 저장/승인 때 현재 원본·대상·버전·권한을 재검사한다.

현재 요청: requestId, careSpaceId, recordId, content, recordedAt, timezone, candidates.
현재 응답: requestId, suggestions. 유형 SCHEDULE/TASK/QUESTION/OBSERVATION/GUIDANCE, 작업 CREATE/UPDATE/RESOLVE.
changes는 문자열 맵이며 엔티티로 직접 복사하지 않는다. 유형별 허용 필드·날짜·상태 전이를 별도로 검증해야 한다.
중복 근거 연결·상충 표시를 현재 계약에 어떻게 담을지는 DECISIONS D04 참조.
timeout: 연결 3초/응답 30초, 자동 재시도 없음. 멱등성·비동기 작업·AI 결과 저장은 도메인 구현에서 설계한다.

## 검증과 개발 운영

기존 명령:
- Docker PostgreSQL: docker compose up -d --wait
- 앱: .\gradlew.bat bootRun
- 검증: .\gradlew.bat check bootJar
- 포맷 수정: .\gradlew.bat spotlessApply
- Docker 없이 AI 계약 테스트: .\gradlew.bat test --tests '*HanmaumAiClientTests'

도메인 검증은 소유자/구성원·제외된 사용자·단일 담당자·버전 충돌·잠금·선택 묶음 롤백·재전송 중복·삭제 전파를 포함한다.
테스트는 합성 기록과 가짜 외부 응답으로 수행한다. 실제 로그인과 운영 배포는 별도 검증한다.

첫 배포의 Lightsail 2 vCPU·4GB/Nginx·Compose 구상은 초기 계획이며 실제 리소스 생성 완료가 아니다.
현재 docker-compose.yml은 로컬 DB용이다. Redis·벡터 DB·메시지 브로커는 초기 구성에 없으며 근거 없이 추가하지 않는다.
개발 dev / 배포 main, PR은 dev 대상으로 팀 승인 규칙을 따른다. 이 패키지는 커밋·PR·배포를 실행하지 않았다.

## 첫 산출물 제안

최신 정책에 맞춘 권한 표, 논리 ERD와 삭제 전파 설명, 핵심 API 초안, 기존 오류/인증 계약과의 차이, FE·AI가 필요한 질문 목록을 먼저 만든다.
그 뒤 로그인→공간→첫 홈에 필요한 최소 도메인부터 구현한다. 아직 결정되지 않은 제품 정책만 분리해 사용자에게 확인한다.

