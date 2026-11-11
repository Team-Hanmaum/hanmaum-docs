# 한마음 FE 개발 채팅 인수인계

작업 레포: C:/dev/Hanmaum/hanmaum-frontend
먼저 [공통 기준](COMMON.md), [충돌과 미정](DECISIONS.md), [Figma 목록](FIGMA.md), [스타일 토큰](TOKENS.md)을 읽는다.
이 문서는 개발 시작 지침이다. 새 채팅에서 현재 파일과 Git 상태를 다시 확인하고 사용자가 승인한 작업 범위를 실행한다.

## 현재 코드 상태

관측 커밋: 35672e8, dev, 2026-10-01 조회 당시 깨끗한 작업 트리.

- [package.json](../../hanmaum-frontend/package.json): Node 24.x, pnpm 10.18.2, React 19.2.8 계열, TypeScript 6.0.2 계열, Vite 8.3.0 계열, Tailwind CSS 4.3.3 계열. 정확한 설치 버전은 pnpm-lock.yaml 기준.
- [vite.config.ts](../../hanmaum-frontend/vite.config.ts): React/Tailwind Vite 플러그인, @ → src 별칭.
- [src/App.tsx](../../hanmaum-frontend/src/App.tsx): 마음 수를 증가시키는 초기 UI PREVIEW 데모. 실제 한마음 로그인/홈 구현으로 간주하지 않는다.
- [src/index.css](../../hanmaum-frontend/src/index.css): Tailwind import만 존재.
- 한마음 토큰·공통 컴포넌트·업무 화면·라우터·서버 상태 라이브러리는 현재 코드/의존성에서 확인되지 않았다.
- ESLint·Prettier·Husky·lint-staged 기반이 있다. 기존 설정을 재생성하거나 패키지를 불필요하게 업그레이드하지 않는다.

## 사용자 요청과 구현 방향

사용자는 FE에서 공통 스타일 토큰과 공통 컴포넌트를 구현한 후 화면 개발을 시작하려 한다.
권장 실행은 토큰 → 기초 컴포넌트 → 로그인/첫 홈 세로 흐름 → 필요 컴포넌트 확장이다. 전체 컴포넌트를 먼저 완성해야만 API 연결을 시작하는 방식은 피한다.

**대화에서 확정된 디자인**
- Noto Sans KR. 초기 Pretendard 선택은 사용하지 않는다.
- 청록 기본 #24746B, 눌림 #1C5F58.
- 오류/주의 행동 기본 #C93625, 눌림 #B83224, 연한 배경 #FFF1ED. 과거 제안 #DC3E26을 현재 확정값으로 사용하지 않는다.
- 390px는 설계 기준 너비다. 실제 웹은 다양한 휴대폰 너비에서 동작해야 하며 프레임의 긴 높이를 고정 viewport 높이로 복제하지 않는다.
- 사용자 제작 로고를 사용한다. 임의 이모지·h. 데모 로고로 대체하지 않는다.
- 홈페이지와 각 탭은 [상세 UI 페이지](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=172-2)의 최신 탭 섹션을 기준으로 한다.
- 와이어프레임·대표 화면 보드는 흐름/과거 참고용이다. 최신 상세 UI와 충돌할 때 과거 보드로 되돌리지 않는다.

## 토큰과 자산

현재 Figma는 Primitives 18 + Color 42 + Layout 34 = 94개 변수, Noto Sans KR 텍스트 스타일 12개다.
정확한 이름·값·alias·CSS 표기는 [TOKENS.md](TOKENS.md) 및 [원본 구조](sources/figma-snapshot.json)에 있다.

- CSS 변수에서 semantic → primitive 연결을 유지한다. Tailwind theme 매핑은 현재 v4 구성에 맞춘다.
- 기존 Figma CSS 이름에는 --hm-*과 소셜의 --color-social-*가 공존한다. 이름을 통일할 경우 매핑과 영향 범위를 기록하고 무음 변경하지 않는다.
- 일반 control 높이 52px, 소셜 로그인 버튼 54px, 최소 터치 영역 44px. 소셜 높이 차이를 오류로 보고 52px로 강제하지 않는다.
- 스타일: Page 26/38 Bold, Hero 22/32 Bold, Section 18/28 Bold, Body 16/26, Reading 16/28, Button 16/24 Bold 등. 단순 임의 px 대신 이름 있는 공통 스타일로 구현한다.
- 로고/Google/Kakao는 Figma의 실제 vector 자산을 재사용한다. 구현 시 추출한 SVG의 viewBox·색상·사용 권리를 확인한다.
- Figma를 코드로 구현할 때는 figma-design-to-code 스킬을 적용하고 해당 화면을 직접 읽는다. 이 패키지의 목록만 보고 픽셀 디자인을 상상하지 않는다.

## 공통 컴포넌트 우선순위 제안

| 단계 | 대상 | 목적 |
| --- | --- | --- |
| 1 | Button, SocialLogin Google/Kakao, Field, IconButton, TextAction | 로그인·기본 폼·공통 행동 |
| 2 | Header, BottomNav, NavItem, TabItem, MenuRow, SectionHeader | 탭과 하위 화면 구조 |
| 3 | Notice, EmptyState, ErrorState, LoadingState, ConfirmDialog, BottomSheet, ActionBar | 정상·빈 화면·오류·확인 단계 |
| 4 | Avatar, FamilyAvatar, FamilyMember, SelectionRow, Badge | 가족/역할·선택 |
| 5 | CareCard, NewsCard, Source, Comparison, Proposal | 돌봄과 소식 핵심 흐름 |

Figma 컴포넌트 세트 25개와 독립 컴포넌트 29개(아이콘 23개 포함)가 존재한다. 전부 서로 다른 라우트나 큰 React 컴포넌트가 필요한 것은 아니다. props와 조합으로 재사용한다. Figma variant의 상태를 실제 이벤트·접근성·비동기 상태로 연결하는 것이 개발 작업이다.

**소셜 로그인은 전용 컴포넌트**
- Google: 401:15428, Kakao: 401:15452.
- Default/Loading/Disabled, 브랜드 아이콘·색·문구를 각각 관리한다. 일반 버튼에 화면별 임의 색상/아이콘을 덧씌우지 않는다.
- 상세 UI 5개 화면 10개 버튼이 전용 인스턴스로 연결되어 있다.
- 부모 기본값은 중앙에서 관리하고, 필요한 경우에만 label을 재정의한다. 불필요한 화면별 기본 문구 복제로 수정 전파를 막지 않는다.
- 기본 라벨: Google로 계속하기 / 카카오로 계속하기.
- 최신 A02는 중앙 큰 로고 + 로그인 안내 + 소셜 버튼 구조다. 제거한 '우리 가족의 돌봄 공간' 초록 안내 카드를 되살리지 않는다.
- 제공사 공식 브랜드 버튼 규격 확인은 구현 시 별도로 수행한다. 현재 시안이 공식 준수 인증을 받았다는 뜻은 아니다.

## 화면과 내비게이션

하단 탭: 홈 / 돌봄 / 소식 / 가족 / 전체.
탭 메인은 하단 내비게이션을, 작업에 집중하는 하위 화면은 Figma의 뒤로/ActionBar 패턴을 따른다. 모든 예외 상태에 하단 바를 기계적으로 붙이지 않는다.

상단 공간명 → 드롭다운 공간 목록 → 다른 공간 이름 탭 시 해당 공간 홈.
'공간 전체 보기' → F26 참여 공간 선택.
F26-A/B/C/D/E/F는 닫힘·열림·로딩·실패·전환 중·전환 실패 상태다. 현재 공간과 목표 공간의 데이터가 섞이지 않게 한다.

공통 라우트/시트/다이얼로그는 진입 탭, careSpaceId, itemId/recordId/requestId, 복귀 위치를 보존하는 방식으로 설계한다.
이 파라미터 이름과 실제 URL 경로는 **구현 제안**이며 공개 API·라우트 최종 명세는 아직 없다.

소식의 AI 제안 수정은 확정 돌봄 항목 수정과 다르다. 화면에 비슷한 날짜 입력기가 있어도 저장 대상·승인 상태를 섞지 않는다.
Figma의 날짜·이름·공간명은 합성 예시다. 이를 제품 기본값 또는 실제 사용자 데이터로 넣지 않는다.

## BE 연동의 현재 기준

브라우저는 Spring에 요청한다. AI 내부 API와 내부 키를 FE에서 호출·보관하지 않는다.

현재 구현된 BE 경로:
- GET /api/auth/csrf → token, headerName 등의 CSRF 정보.
- /oauth2/authorization/google, /oauth2/authorization/kakao → 로그인 시작. OAuth 프로필/콘솔 키 설정과 실제 로그인 검증은 별도.
- POST /api/auth/logout → 세션 종료, 성공 204.
- 나머지 회원/공간/기록/제안/항목 공개 API는 아직 구현 전.

요청에는 credentials: 'include'. 쓰기 요청은 CSRF 응답의 headerName에 token을 넣는다. 로그인/로그아웃 후 토큰을 다시 받는다. 로컬 FE와 BE의 호스트를 localhost로 통일한다.
현재 오류는 code/message/fieldErrors. 통일된 성공 envelope는 미정이다. 204에 JSON 파싱을 강제하지 않는다.
401 인증 만료와 403 권한/CSRF 오류를 섞어 처리하지 않는다. 공개 오류 세분화는 BE와 합의한다.

API 명세 합의 전에는 Mock 응답을 명시적으로 사용한다. 실제 저장·실제 로그인 완료인 것처럼 처리하지 않는다.

## 상태별 검증 기준

- 원본 저장 성공/실패와 AI 분석 성공/실패를 분리.
- 구성원 버튼은 '요청', 소유자는 최종 반영. 요청 후 현재 값은 유지.
- 답변 제출 후 미확인 상태 유지, 승인 후에만 확인됨.
- 확정 실패는 재시도 가능, 결과 미확인은 먼저 결과 조회. 중복 POST 금지.
- 잠금·버전 충돌 때 입력과 근거를 보존하며 재확인. 조용한 덮어쓰기 금지.
- 권한 상실·로그아웃·공간 전환 뒤 이전 공간의 민감한 캐시/화면을 노출하지 않음.
- 홈 전체 실패와 일부 영역 실패를 구분. 이미 조회된 영역을 불필요하게 지우지 않음.
- 작은 휴대폰 너비·긴 한국어·키보드/포커스·터치 영역·스크롤과 고정 바의 겹침 확인.
- 새 날짜/담당자/완료 상태를 AI가 추측한 사실처럼 넣지 않음.

## 첫 작업 제안과 완료 기준

1. 현 repo 구조·package manager·기존 설정과 이 문서의 차이를 보고한다.
2. Figma 토큰과 자산을 읽어 token/typography 기반을 구현한다.
3. 기본 Button/Field/소셜 로그인/Header 등 첫 흐름에 필요한 컴포넌트를 구현하고 상태 예시를 확인한다.
4. A02 로그인 등 대표 화면에서 실제 조합을 검증한 뒤 나머지 화면에 확장한다.
5. 변경 범위에 맞게 기존 check/build를 실행하고, 검증하지 않은 OAuth/BE 연동을 따로 보고한다.

기존 명령:
- pnpm install --frozen-lockfile
- pnpm check
- pnpm build
- pnpm dev

이 인수인계에서는 이 명령을 실행하지 않았다. 현재 테스트 러너가 없는 상태이므로 추가할 때는 상호작용·권한 분기·요청 중복 방지처럼 의미 있는 검증 범위를 기준으로 결정한다.

