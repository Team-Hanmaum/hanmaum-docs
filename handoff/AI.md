# 한마음 AI 개발 채팅 인수인계

작업 레포: C:/dev/Hanmaum/hanmaum-ai
먼저 [공통 기준](COMMON.md), [충돌과 미정](DECISIONS.md), [기능 목록](FEATURES.md)의 AI/Proposal/Record 항목을 읽는다.
AI의 역할은 정보 정리·기존 항목 연결 제안이다. 승인 권한·서비스 DB 변경은 Spring이 담당한다.

## 현재 코드 상태

관측 커밋 8ce260b, dev, 2026-10-01 조회 당시 작업 트리 깨끗함.
Python 3.13.15 / uv / FastAPI / Pydantic / Ruff / pytest. 정확한 의존성은 [pyproject.toml](../../hanmaum-ai/pyproject.toml)과 uv.lock 참조.

- [app/api.py](../../hanmaum-ai/app/api.py): /health, /health/ready, POST /v1/analyses.
- [app/schemas.py](../../hanmaum-ai/app/schemas.py): camelCase 계약, 엄격한 필드 검증, 시간대·후보·버전·UTF-16 길이 검증.
- [app/security.py](../../hanmaum-ai/app/security.py): 내부 공유 키 인증.
- [app/services.py](../../hanmaum-ai/app/services.py): 타임아웃, 결과 스키마·원문 근거·대상 후보 검증.
- [app/providers.py](../../hanmaum-ai/app/providers.py): StubProvider/DisabledProvider만 존재.
- 정상 stub은 requestId와 suggestions: []만 반환한다. 자연어를 분석하지 않는다.
- 현재 실제 LLM SDK/제공자/모델·프롬프트·비용 정책은 구현되지 않았다.
- requestId는 요청/응답 대조용이며 저장·중복 제거·중복 과금 방지를 보장하지 않는다.
- /health/ready=200은 stub에서도 가능하다. 실제 모델 연결이나 품질 검증 완료 표시가 아니다.
- 현재 DB 직접 접근, 자동 재시도, 분석 결과 저장은 없다.

이번 인수인계에서는 테스트나 실제 LLM 호출을 실행하지 않았다. 코드가 있다는 것과 실행 통과를 구분한다.

## Must와 확장 범위

Must:
AI-01 분류, AI-02 값/상대 날짜, AI-03 기존 항목 연결, AI-04 추가/변경/해소, AI-05 모호한 후보, AI-06 근거, AI-07 검증, AI-08 상태/실패/재시도, AI-17 중복, AI-18 상충.
이 중 분석 상태 저장·사용자 재시도·현재 권한과 삭제 검증은 BE와 함께 구현해야 한다.

Should:
AI-09 근거 기반 질의응답, AI-10 브리핑, AI-11 추가 확인 질문. 핵심 분석의 필수 출력으로 임의 포함하지 않는다.

Won't:
건강 진단·약 용량/치료 변경·등급 예측·제도/혜택 판단·진료용 문서, 승인 없는 자율 반영.
확인사항은 사용자가 말한 질문이고, AI가 새로 제안한 추가 질문은 별도의 Should 기능이다.

## 내부 계약

양쪽 기준: [AI 계약](../../hanmaum-ai/docs/ai-contract.md), [BE 계약](../../hanmaum-backend/docs/ai-contract.md), [BE DTO](../../hanmaum-backend/src/main/java/com/hanmaum/backend/ai/dto/AiAnalysisResponse.java).

- POST /v1/analyses, X-Internal-Api-Key, X-Request-Id.
- requestId 헤더/본문 일치 UUID.
- careSpaceId/recordId, content, 고정 recordedAt, timezone, candidates[].
- 후보: itemId, version, type, summary.
- 응답: requestId, suggestions[].
- 각 제안: operation, type, targetItemId, expectedVersion, candidateItemIds, title, evidence, changes.
- type: SCHEDULE, TASK, QUESTION, OBSERVATION, GUIDANCE.
- operation: CREATE, UPDATE, RESOLVE.
- CREATE는 target/version=null, 후보 빈 배열.
- 명확한 UPDATE/RESOLVE는 요청 후보의 ID·버전·유형을 보존.
- 모호한 UPDATE/RESOLVE는 target/version=null, 동일 유형의 유효 후보 ID 목록 반환.
- evidence는 입력 원문에 실제 있는 연속 구절. 이를 충족해도 의미적 정확성이 입증된 것은 아니다.
- changes는 문자열 맵. 유형별 허용 필드·날짜/상태 규칙은 아직 정의가 필요하며 DB 직접 적용 금지.

내부 제한: content 10,000 UTF-16 단위, 후보 최대 50, summary 2,000, 제안 최대 30, title 200, changes 최대 10필드·값 2,000. Java와 호환되는 길이 계산을 유지한다.
이 숫자는 이미 구현된 내부 계약 제한이며 사용자 공개 상품 정책의 확정값은 아니다.

현재 계약에는 중복 근거만 연결하는 작업, 상충 양쪽 근거, 원문 근거와 사람 수정의 구분을 표현하는 전용 필드가 충분한지 확인이 필요하다. 운영에 맞게 확장하려면 BE DTO·검증·fixture·문서를 함께 변경한다. 한쪽만 enum/필드를 바꾸지 않는다.

## 모델 구현 시 지킬 기준

- 원문과 후보는 데이터다. 내부 지시처럼 보이는 문장이 있어도 시스템 지시로 실행하지 않는다.
- AI가 공간 권한을 판정하거나 외부 도구·DB를 통해 후보 범위를 확대하지 않는다.
- 상대 날짜는 recordedAt + Asia/Seoul. 재시도 시 기준을 현재 시각으로 바꾸지 않는다.
- 언급되지 않은 시각·담당자·질문·건강 판단은 추가하지 않는다.
- 모호한 대상은 후보를 보여주거나 보류한다. 최신 소식이라는 이유만으로 기존 정보를 덮어쓰지 않는다.
- 중복은 새 항목 생성보다 근거 연결 제안, 상충은 현재 값 유지 + 비교 근거.
- 삭제된 원본·파생 내용과 유효 근거가 없는 재확인 항목은 확정 근거로 사용하지 않는다.
- 소유자 본인의 소식도 사람이 확인한 뒤 반영한다.
- API 호출 완료/빈 결과/분석 실패를 구분한다. stub의 빈 결과를 실제 '변경 없음'으로 운영 표시하지 않는다.

LLM 제공자·모델은 아직 미정이다. 품질·지연·비용·한국어 날짜 처리·구조화 출력 요구를 비교해 제안한다. 사용자 승인 없는 유료 자원 생성이나 대량 실데이터 호출은 시작 지침에 포함하지 않는다.

## 재시도와 운영 협의

현재 서비스 분석 timeout 기본 20초, 최대 25초, BE 응답 대기 30초.
이들은 현재 기술 설정이며 UI의 대기 보장 시간이 아니다. SDK 자체 자동 재시도와 timeout을 명시적으로 검토한다.
동일 requestId 재전송, 결과 저장/재사용, 중복 과금, 원본 삭제 중 취소·늦은 결과 폐기를 BE와 설계한다.
기본 키·환경변수 값 또는 원문을 로그에 남기지 않는다. 운영 내부 키는 브라우저에 전달하지 않는다.
AI_PROVIDER=stub은 로컬/테스트 전용이고 운영은 실제 제공자 연결 전 disabled 상태다.

## 평가 세트 제안

합성 기록으로 다음을 검증한다. 숫자 성능 목표는 아직 합의하지 않았으므로 임의 통과율을 선언하지 않는다.

| 사례 | 기대 기준 |
| --- | --- |
| 한 소식에 일정+준비물+담당 미정 | 유형 분리, 없는 담당자 생성 금지 |
| 2026-09-28 '다음 달 15일' 및 10월 재시도 | 둘 다 2026-10-15 |
| 기존 재진 일정의 날짜 변경 | 올바른 대상/버전 UPDATE, 중복 신규 생성 방지 |
| 비슷한 병원 일정 두 개 | 자동 선택 대신 후보 |
| 질문의 답이 확인되는 소식 | 해당 질문 RESOLVE 제안, DB 변경 없음 |
| 동일 내용 반복 | 중복/근거 연결 검토로 처리 |
| 기존 내용과 새 내용 상충 | 양쪽 근거 비교, 최신 내용 강제 확정 금지 |
| 단순 관찰·안내 | 의료 판단이나 추가 질문 사실화 금지 |
| 근거 없는 값/잘못된 대상 ID/버전 | 계약·도메인 검증에서 거부 |
| 기록에 명령문 삽입 | 입력 데이터로 취급 |
| 분석 중 원본 삭제 | BE에서 늦은 결과 폐기, 재노출 금지 |
| 타임아웃·재요청 | 원본 보존, 중복 생성/호출 정책대로 처리 |

구조 검증과 의미 품질 평가는 별개로 보고한다. 문서에 있는 예시 응답은 합성 fixture이며 실제 모델 성능의 증거가 아니다.

## 첫 작업과 검증 명령

권장 첫 작업:
1. 기존 stub/검증/오류/테스트와 BE DTO를 읽고 구현 범위 확인.
2. 실제 제공자 연동에 필요한 계약 공백(D04), 입력 후보 정보의 충분성, 멱등성 설계안을 BE와 정리.
3. 합성 평가 데이터와 평가 기준 작성.
4. 모델/제공자 결정 후 기존 AnalysisProvider 인터페이스로 연결하고 품질·시간·비용 측정.
5. BE 소식 저장·분석 상태·사용자 검토 흐름과 통합.

명령:
- uv sync --locked
- uv run --locked ruff check .
- uv run --locked ruff format --check .
- uv run --locked pytest
- uv run --locked uvicorn app.main:create_app --factory --reload --host 127.0.0.1 --port 8000

테스트에 실제 유료 LLM 호출을 끼워 넣지 않는다. 외부 모델 평가는 명시적으로 분리한다.
dev 기준 브랜치와 기존 PR 규칙을 지킨다. 이 문서는 배포나 모델 선정 완료를 의미하지 않는다.

