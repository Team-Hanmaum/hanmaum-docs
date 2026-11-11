# 한마음 API 안내

[Notion에서 API 명세서 보기](https://seen-airship-65a.notion.site/API-3ec0f25ea138807cad15e07696472145?source=copy_link)

## 기본 주소와 인증

공개 업무 API의 기본 경로는 `/api`다. 운영 기본 주소는 `https://api.hanmaum.site/api`이고, 로컬 BE 기본 주소는 `http://localhost:8080/api`다.

- 인증이 필요한 요청은 서버 세션 쿠키를 사용한다. 프론트엔드는 브라우저 요청에 `credentials: "include"`를 설정한다.
- CSRF 검증이 필요한 변경 요청에는 `GET /api/auth/csrf`에서 받은 헤더 이름과 토큰을 사용한다. 로그인·로그아웃 후에는 다음 변경 요청 전에 토큰을 다시 발급받는다.

## 공통 응답

일반 JSON 응답은 `success`, `code`, `message`, `data`, `errors`, `timestamp`를 포함한다. 프론트엔드는 HTTP 상태와 `code`로 결과를 구분하고, 필드 오류는 `errors`에서 읽는다. OAuth 리다이렉트, CSRF 발급 성공, 본문 없는 `204 No Content`는 각 경로의 응답 규격을 따른다.

## 주요 API 영역

| 영역 | 역할 |
| --- | --- |
| Auth · User | 소셜 로그인, 세션·CSRF, 내 정보, 회원 탈퇴 |
| CareSpace | 돌봄 공간과 구성원 관리, 초대·참여, 소유권 이전 |
| Record · Analysis | 소식 원본 등록·조회·삭제, 분석 상태 조회와 재시도 |
| Proposal | AI 제안과 변경 요청의 검토·반영 |
| CareItem | 돌봄 항목 등록·변경·삭제, 답변, 편집 잠금, 근거·이력 |
| Dashboard | 일정·할 일·미확인 질문·최근 변경 요약 |
| Sharing | 공유 링크 발급과 현재 참여자의 공유 항목 조회 |

## 상세 명세

- 요청·응답 형식, 공통 오류, 권한과 도메인별 계약은 [BE 공개 API 공통 규격](https://github.com/Team-Hanmaum/hanmaum-backend/blob/dev/docs/api-conventions.md)과 [BE 상세 문서](https://github.com/Team-Hanmaum/hanmaum-backend/tree/dev/docs)를 참고한다.
- 배포된 API의 경로와 스키마는 [운영 Swagger](https://api.hanmaum.site/swagger-ui.html)에서 확인한다. 로컬에서는 BE 실행 후 `http://localhost:8080/swagger-ui/index.html`로 접속한다.
