# 한마음 Figma 구현 참조

기준일: 2026-10-01 KST. 파일: [한마음](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=172-2). 이번 조회는 파일 구조·화면 이름·컴포넌트·변수·주석을 조사한 것으로, 모든 화면의 픽셀과 모든 버튼 동작을 다시 검수한 결과는 아니다.

## 읽는 순서

1. [상세 UI 페이지](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=172-2)의 담당 탭 섹션에서 현재 화면과 좌우 흐름을 확인한다.
2. [공통 컴포넌트 v1](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=176-3)과 [토큰 조회표](TOKENS.md)를 함께 본다.
3. 화면의 정상·빈 화면·오류·잠금·승인 대기 분기를 같은 기능의 상태로 묶고, [정책 충돌/미정](DECISIONS.md)과 대조한다.
4. 구현할 화면의 Figma를 다시 조회해 최근 변경을 반영한다. 아래 목록은 기준일의 색인이다.

[와이어프레임](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=0-1)은 흐름 설계의 배경 자료다. [초기 대표 화면 보드](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=186-518)보다 탭별 상세 UI를 우선 참고한다. 예시 이름·날짜·소식·카운트는 실제 데이터나 기본값 정책이 아니다. 사용자 결정에 따라 프로토타입 연결은 구현 요건이 아니다.

## 섹션과 화면 예시 개수

아래 **356개는 화면·상태 예시와 부분 프레임의 합**이다. 356개의 라우트 또는 독립 기능을 만들라는 의미가 아니다. 모달·시트·로딩·권한별 예시는 공통 화면과 컴포넌트 상태로 재사용한다. 너비 390px 화면 이름 규칙으로 추출했으며, 가족의 상단 공간 메뉴 부분 프레임 6개를 추가로 포함했다.

| 섹션 | 화면·상태 예시 | 포함된 보드 |
| --- | ---: | --- |
| [01 · 온보딩 → 첫 홈](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=363-16643) | 11 | [한마음 · 상세 UI 2차 · 온보딩과 첫 홈](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=206-730) |
| [02 · 홈](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=363-16644) | 7 | [한마음 · 상세 UI 3차 · 홈](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=213-1219) |
| [03 · 돌봄](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=363-16645) | 163 | [한마음 · 상세 UI 4차 · 돌봄](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=222-1682)<br>[한마음 · 상세 UI · 돌봄 전체 하위 흐름](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=260-5576) |
| [04 · 소식](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=363-16646) | 76 | [한마음 · 상세 UI 5차 · 소식](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=229-2803)<br>[한마음 · 상세 UI · 소식 하위 흐름](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=301-12386) |
| [05 · 가족](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=363-16647) | 32 | [한마음 · 상세 UI 6차 · 가족](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=246-4285) |
| [06 · 전체](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=363-16648) | 67 | [한마음 · 상세 UI · 전체 탭과 계정·공간 관리](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=319-14276) |

## 최근 합의한 구현 포인트

- 폰트는 Noto Sans KR이다. 초기 Pretendard 설명은 현재 기준이 아니다.
- 홈 헤더의 로고와 '한마음'은 공통 헤더·브랜드 자산에서 가져온다. 참여 공간명은 실제 선택 공간 데이터로 전달한다. Figma 예시 '엄마의 돌봄'을 고정 문자열로 구현하지 않는다.
- 상단 공간 메뉴는 선택 공간으로 전환하고, '공간 전체 보기'에서 F26으로 이어진다. 메뉴만 보여 주는 부분 프레임을 별도 전체 화면으로 구현하지 않는다.
- 로그인은 중앙의 큰 로고와 Google·카카오 전용 소셜 컴포넌트를 사용한다. 기존 초록 안내 카드를 복구하지 않는다.
- 일반 Button과 SocialLogin은 별도 컴포넌트다. 색상·아이콘·라벨·상태를 전용 컴포넌트에 모은다.
- 상세 화면·작성 화면·시트에 하단 내비게이션을 기계적으로 추가하지 않는다. 해당 화면의 흐름과 주석을 확인한다. 긴 디자인 프레임은 내용 전체를 보여 주는 용도이며, 브라우저에서는 스크롤과 고정 영역을 구분한다.
- UI 예시가 있더라도 기능 DB의 Should/Could를 Must로 승격하지 않는다. 서로 다른 탭에서 진입하는 같은 상세 화면은 공유할 수 있다.

| 자주 찾는 대상 | 노드 |
| --- | --- |
| 로그인 · 최신 중앙 로고 화면 | [206:749](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=206-749) |
| 일반 홈 | [213:1233](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=213-1233) |
| 돌봄 메인 | [223:1684](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=223-1684) |
| 일정 추가 | [261:5578](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=261-5578) |
| 할 일 추가 | [261:5708](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=261-5708) |
| 질문 추가 | [261:5778](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=261-5778) |
| 소식 작성 | [230:3008](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=230-3008) |
| 소식 저장 후 분석 중 | [231:3021](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=231-3021) |
| 구성원 제안 검토 | [234:3452](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=234-3452) |
| 소유자 제안 검토 | [234:3482](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=234-3482) |
| 승인 대기 | [234:3657](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=234-3657) |
| 반영 완료 | [234:3737](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=234-3737) |
| 가족 · 소유자 | [247:4287](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=247-4287) |
| 가족 · 구성원 | [247:4415](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=247-4415) |
| 공간 메뉴 · 닫힘 (헤더만) | [375:15035](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=375-15035) |
| 공간 메뉴 · 열림 | [375:15054](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=375-15054) |
| 참여 공간 전체 선택 | [254:5548](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=254-5548) |
| 전체 · 소유자 | [320:14278](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=320-14278) |
| 전체 · 구성원 | [320:14482](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=320-14482) |
| 공통 헤더 세트 | [180:147](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=180-147) |
| 브랜드 심볼 | [340:16402](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=340-16402) |
| Google 로그인 세트 | [401:15428](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=401-15428) |
| 카카오 로그인 세트 | [401:15452](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=401-15452) |

## 공통 컴포넌트 세트 25개

변형은 디자인에 실제 존재하는 이름이다. 코드 API는 기능에 맞게 정하되 이 상태를 빠뜨리지 않는다. 예시 텍스트를 전역으로 고정하지 말고, 공통 기본값과 화면별 데이터/문구를 구분한다.

| 컴포넌트 | 변형 수 | 변형 축과 값 |
| --- | ---: | --- |
| [Hanmaum/Badge](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=177-289) | 3 | Tone: Neutral / Brand / Error |
| [Hanmaum/Avatar](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=177-298) | 2 | Emphasis: Default / Self |
| [Hanmaum/Spinner](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=177-312) | 3 | Tone: Brand / Inverse / Secondary |
| [Hanmaum/Button](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=178-177) | 24 | State: Default / Hover / Pressed / Focus / Disabled / Loading<br>Style: Primary / Secondary / Outline / Destructive |
| [Hanmaum/SocialLogin/Google](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=401-15428) | 3 | State: Default / Loading / Disabled |
| [Hanmaum/SocialLogin/Kakao](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=401-15452) | 3 | State: Default / Loading / Disabled |
| [Hanmaum/TextAction](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=178-198) | 8 | State: Default / Pressed / Focus / Disabled<br>Tone: Default / Danger |
| [Hanmaum/IconButton](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=178-215) | 4 | State: Default / Pressed / Focus / Disabled |
| [Hanmaum/Field](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=179-171) | 10 | State: Empty / Filled / Focus / Error / Disabled<br>Type: Single / Multiline |
| [Hanmaum/SelectionRow](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=179-249) | 12 | Control: Checkbox / Radio<br>State: Default / Focus / Disabled<br>Selected: No / Yes |
| [Hanmaum/TabItem](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=179-263) | 4 | State: Default / Selected / Focus / Disabled |
| [Hanmaum/Header](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=180-147) | 2 | Type: Main / Back |
| [Hanmaum/NavItem](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=180-168) | 3 | State: Default / Selected / Focus |
| [Hanmaum/BottomNav](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=180-354) | 5 | Active: Home / Care / News / Family / All |
| [Hanmaum/MenuRow](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=180-385) | 2 | Tone: Default / Danger |
| [Hanmaum/FamilyMember](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=180-406) | 2 | Role: Owner / Member |
| [Hanmaum/CareCard](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=182-422) | 3 | State: Default / NeedsCheck / Locked |
| [Hanmaum/NewsCard](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=182-511) | 6 | Status: Analyzing / Review / Pending / Applied / NoChange / Failed |
| [Hanmaum/Source](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=183-352) | 2 | State: Available / Redacted |
| [Hanmaum/Proposal](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=183-415) | 2 | Selected: No / Yes |
| [Hanmaum/Notice](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=182-360) | 3 | Tone: Info / Neutral / Error |
| [Hanmaum/ErrorState](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=184-430) | 3 | Reason: LoadError / Offline / NoPermission |
| [Hanmaum/LoadingState](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=184-455) | 2 | Type: Initial / More |
| [Hanmaum/ConfirmDialog](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=184-494) | 2 | Intent: Default / Destructive |
| [Hanmaum/ActionBar](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=184-552) | 2 | Type: Single / WithSecondary |

## 단독 컴포넌트 29개

컴포넌트 세트와 별도로 아이콘 23개 및 브랜드·구성 요소 6개가 있다. 아래 숫자는 코드 컴포넌트 수를 강제하지 않는다.

| 컴포넌트 | 참조 |
| --- | --- |
| Hanmaum/Icon/Home | [177:161](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=177-161) |
| Hanmaum/Icon/Care | [177:166](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=177-166) |
| Hanmaum/Icon/News | [177:172](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=177-172) |
| Hanmaum/Icon/Family | [177:178](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=177-178) |
| Hanmaum/Icon/Grid | [177:184](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=177-184) |
| Hanmaum/Icon/ChevronRight | [177:192](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=177-192) |
| Hanmaum/Icon/ChevronLeft | [177:197](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=177-197) |
| Hanmaum/Icon/ChevronDown | [177:203](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=177-203) |
| Hanmaum/Icon/Plus | [177:208](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=177-208) |
| Hanmaum/Icon/Check | [177:213](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=177-213) |
| Hanmaum/Icon/Close | [177:218](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=177-218) |
| Hanmaum/Icon/Edit | [177:223](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=177-223) |
| Hanmaum/Icon/Info | [177:228](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=177-228) |
| Hanmaum/Icon/Alert | [177:234](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=177-234) |
| Hanmaum/Icon/Lock | [177:241](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=177-241) |
| Hanmaum/Icon/Clock | [177:247](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=177-247) |
| Hanmaum/Icon/Link | [177:253](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=177-253) |
| Hanmaum/Icon/Person | [177:258](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=177-258) |
| Hanmaum/Icon/ArrowRight | [177:264](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=177-264) |
| Hanmaum/Icon/Refresh | [177:269](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=177-269) |
| Hanmaum/Icon/Spinner | [177:275](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=177-275) |
| Hanmaum/Icon/Google | [396:15404](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=396-15404) |
| Hanmaum/Icon/Kakao | [396:15413](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=396-15413) |
| Hanmaum/Brand/Symbol | [340:16402](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=340-16402) |
| Hanmaum/SectionHeader | [180:359](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=180-359) |
| Hanmaum/FamilyAvatar | [196:500](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=196-500) |
| Hanmaum/Comparison | [183:327](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=183-327) |
| Hanmaum/EmptyState | [184:373](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=184-373) |
| Hanmaum/BottomSheet | [184:499](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=184-499) |

## 전체 화면·상태 예시 색인

각 이름은 Figma 노드의 실제 이름을 그대로 기록했다. 경고·분기 예시도 포함되어 있으므로 이름을 API 경로나 정책으로 그대로 옮기지 않는다.

### 01 · 온보딩 → 첫 홈

| 화면·상태 이름 | 노드 |
| --- | --- |
| HM-ON-A01 · 가치 먼저 보기 | [206:744](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=206-744) |
| HM-ON-A02 · 간편 로그인 | [206:749](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=206-749) |
| HM-ON-A03 · 기록 전 정보 안내 | [206:754](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=206-754) |
| HM-ON-A04 · 돌봄 공간 만들기 | [206:759](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=206-759) |
| HM-ON-A05 · 첫 홈 · 기록 없음 | [206:764](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=206-764) |
| HM-ON-B01 · 초대 링크로 시작 | [206:773](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=206-773) |
| HM-ON-B02 · 첫 홈 · 가족과 함께 | [206:778](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=206-778) |
| HM-ON-C01 · 로그인 실패 | [206:787](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=206-787) |
| HM-ON-C02 · 초대 링크 만료 | [206:792](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=206-792) |
| HM-ON-C03 · 사용할 수 없는 초대 | [206:797](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=206-797) |
| HM-ON-C04 · 공간 만들기 실패 | [206:802](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=206-802) |

### 02 · 홈

| 화면·상태 이름 | 노드 |
| --- | --- |
| HM-HOME-H01 · 일반 홈 | [213:1233](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=213-1233) |
| HM-HOME-H04 · 최근 확정된 변경 | [213:1238](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=213-1238) |
| HM-HOME-H02 · 첫 빈 홈 | [213:1248](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=213-1248) |
| HM-HOME-H03 · 초대 참여 후 홈 | [213:1253](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=213-1253) |
| HM-HOME-H05 · 첫 로딩 | [385:15045](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=385-15045) |
| HM-HOME-H06 · 전체 조회 실패 | [385:15116](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=385-15116) |
| HM-HOME-H07 · 일정 영역 조회 실패 | [385:15182](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=385-15182) |

### 03 · 돌봄

| 화면·상태 이름 | 노드 |
| --- | --- |
| C01 · 돌봄 메인 · 일정 | [223:1684](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=223-1684) |
| C02 · 일정 상세 · 구성원 | [223:1804](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=223-1804) |
| C03 · 지난 일정 | [223:1908](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=223-1908) |
| C04 · 완료·취소된 일정 | [223:2001](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=223-2001) |
| C05 · 할 일 목록 | [223:2090](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=223-2090) |
| C06 · 할 일 상세 · 다른 구성원 | [223:2201](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=223-2201) |
| C07 · 할 일 상세 · 담당자 | [223:2305](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=223-2305) |
| C08 · 질문 목록 | [223:2409](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=223-2409) |
| C09 · 질문 상세 | [223:2495](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=223-2495) |
| C10 · 관찰 목록 | [223:2600](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=223-2600) |
| C11 · 관찰 상세 | [223:2690](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=223-2690) |
| C12 · 안내 목록 | [223:2774](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=223-2774) |
| C13 · 안내 상세 | [223:2861](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=223-2861) |
| C14 · 돌봄 · 빈 일정 | [223:2947](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=223-2947) |
| C15 · 돌봄 · 불러오기 실패 | [223:3026](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=223-3026) |
| C16 · 일정 상세 · 편집 잠금 | [223:3102](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=223-3102) |
| C17 · 일정 상세 · 반영 후 | [236:4254](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=236-4254) |
| C18 · 일정 추가 · 입력 전 | [261:5578](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=261-5578) |
| C19 · 일정 추가 · 입력 예시 | [261:5646](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=261-5646) |
| C22 · 날짜 선택 | [262:5738](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=262-5738) |
| C23 · 시간 선택 | [262:5870](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=262-5870) |
| C25 · 일정 등록 요청 · 내용 확인 | [262:5979](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=262-5979) |
| C31 · 일정 등록 요청 · 승인 대기 | [262:6246](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=262-6246) |
| C28 · 일정 등록 · 소유자 확인 | [262:6023](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=262-6023) |
| C34 · 일정 등록 완료 | [262:6324](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=262-6324) |
| C37 · 등록한 일정 상세 | [263:6272](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=263-6272) |
| C20 · 할 일 추가 | [261:5708](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=261-5708) |
| C24 · 담당자 선택 | [262:5927](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=262-5927) |
| C26 · 할 일 등록 요청 · 내용 확인 | [262:6067](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=262-6067) |
| C32 · 할 일 등록 요청 · 승인 대기 | [262:6393](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=262-6393) |
| C29 · 할 일 등록 · 소유자 확인 | [262:6115](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=262-6115) |
| C35 · 할 일 등록 완료 | [262:6474](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=262-6474) |
| C38 · 등록한 할 일 상세 | [263:6361](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=263-6361) |
| C21 · 질문 추가 | [261:5778](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=261-5778) |
| C27 · 질문 등록 요청 · 내용 확인 | [262:6162](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=262-6162) |
| C33 · 질문 등록 요청 · 승인 대기 | [262:6546](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=262-6546) |
| C30 · 질문 등록 · 소유자 확인 | [262:6204](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=262-6204) |
| C36 · 질문 등록 완료 | [262:6621](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=262-6621) |
| C39 · 등록한 질문 상세 | [263:6453](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=263-6453) |
| M01 · 항목 관리 · 소유자 | [276:6542](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=276-6542) |
| MT01 · 할 일 관리 · 소유자 | [286:10632](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=286-10632) |
| MQ01 · 질문 관리 · 소유자 | [286:10840](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=286-10840) |
| MO01 · 관찰 관리 · 소유자 | [286:11028](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=286-11028) |
| MG01 · 안내 관리 · 소유자 | [286:11196](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=286-11196) |
| M02 · 항목 관리 · 구성원 | [276:6648](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=276-6648) |
| MT02 · 할 일 관리 · 구성원 | [286:10736](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=286-10736) |
| MQ02 · 질문 관리 · 구성원 | [286:10934](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=286-10934) |
| MO02 · 관찰 관리 · 구성원 | [286:11112](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=286-11112) |
| MG02 · 안내 관리 · 구성원 | [286:11280](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=286-11280) |
| MS03 · 완료·취소한 일정 관리 | [288:12543](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=288-12543) |
| MT03 · 완료한 할 일 관리 | [288:12344](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=288-12344) |
| MQ03 · 확인된 질문 관리 | [288:12449](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=288-12449) |
| M03 · 일정 수정 | [276:6754](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=276-6754) |
| M04 · 일정 변경 확인 · 소유자 | [276:6835](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=276-6835) |
| M05 · 일정 변경 확인 · 구성원 | [276:6882](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=276-6882) |
| M06 · 일정 변경을 요청했어요 | [276:6929](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=276-6929) |
| M07 · 일정 수정 후 상세 | [276:7006](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=276-7006) |
| M08 · 할 일 수정 | [276:7105](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=276-7105) |
| M09 · 할 일 변경 확인 · 소유자 | [276:7183](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=276-7183) |
| M10 · 할 일 변경 확인 · 구성원 | [276:7230](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=276-7230) |
| M11 · 할 일 변경을 요청했어요 | [276:7277](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=276-7277) |
| M12 · 할 일 수정 후 상세 | [276:7354](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=276-7354) |
| M13 · 질문 수정 | [276:7453](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=276-7453) |
| M14 · 질문 변경 확인 · 소유자 | [276:7507](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=276-7507) |
| M15 · 질문 변경 확인 · 구성원 | [276:7554](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=276-7554) |
| M16 · 질문 변경을 요청했어요 | [276:7601](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=276-7601) |
| M17 · 질문 수정 후 상세 | [276:7678](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=276-7678) |
| M18 · 관찰 수정 | [276:7774](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=276-7774) |
| M19 · 관찰 변경 확인 · 소유자 | [276:7821](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=276-7821) |
| M20 · 관찰 변경 확인 · 구성원 | [276:7867](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=276-7867) |
| M21 · 관찰 변경을 요청했어요 | [276:7913](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=276-7913) |
| M22 · 관찰 수정 후 상세 | [276:7989](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=276-7989) |
| M23 · 안내 수정 | [276:8085](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=276-8085) |
| M24 · 안내 변경 확인 · 소유자 | [276:8132](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=276-8132) |
| M25 · 안내 변경 확인 · 구성원 | [276:8178](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=276-8178) |
| M26 · 안내 변경을 요청했어요 | [276:8224](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=276-8224) |
| M27 · 안내 수정 후 상세 | [276:8301](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=276-8301) |
| A01 · 담당자 변경 | [277:7887](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=277-7887) |
| A02 · 담당자 변경 확인 · 소유자 | [277:7946](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=277-7946) |
| A03 · 담당자 변경 확인 · 구성원 | [277:7991](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=277-7991) |
| A04 · 담당자 변경을 요청했어요 | [277:8036](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=277-8036) |
| A05 · 담당자 변경 후 상세 | [277:8113](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=277-8113) |
| TA01 · 담당자 미지정 할 일 | [288:12254](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=288-12254) |
| S01 · 일정 상태 선택 | [278:8125](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=278-8125) |
| S02 · 일정 완료 확인 · 소유자 | [278:8170](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=278-8170) |
| S03 · 일정 취소 확인 · 소유자 | [278:8212](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=278-8212) |
| S04 · 일정 완료 확인 · 구성원 | [278:8254](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=278-8254) |
| S05 · 일정 상태 변경을 요청했어요 | [278:8296](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=278-8296) |
| S06 · 완료된 일정 상세 | [278:8373](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=278-8373) |
| S07 · 취소된 일정 상세 | [278:8468](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=278-8468) |
| T01 · 할 일 진행 중 | [278:8563](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=278-8563) |
| T02 · 할 일 완료 | [278:8662](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=278-8662) |
| T03 · 할 일 진행 중 · 완료 취소 | [278:8761](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=278-8761) |
| T04 · 다른 가족의 할 일 · 상태 선택 | [278:8860](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=278-8860) |
| T05 · 할 일 상태 요청 확인 | [278:8905](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=278-8905) |
| T06 · 할 일 상태 변경을 요청했어요 | [278:8947](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=278-8947) |
| Q01 · 답변 남기기 | [280:8796](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=280-8796) |
| Q02 · 답변 확인 요청 · 내용 확인 | [280:8845](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=280-8845) |
| Q03 · 답변 제출 후 · 미확인 질문 | [280:8884](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=280-8884) |
| Q04 · 답변 요청 상태 | [280:8972](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=280-8972) |
| Q05 · 답변·근거 확인 · 소유자 | [280:9043](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=280-9043) |
| Q06 · 확인된 질문 상세 | [280:9085](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=280-9085) |
| Q07 · 답변 요청 · 미적용 | [280:9176](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=280-9176) |
| SH01 · 공유 카드 미리보기 | [281:9111](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=281-9111) |
| SH02 · 외부 앱 · 수신자 선택 | [281:9152](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=281-9152) |
| SH03 · 공유 화면에서 복귀 | [281:9176](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=281-9176) |
| SH04 · 공유 취소 | [281:9208](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=281-9208) |
| SH05 · 공유 실행 실패 | [281:9231](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=281-9231) |
| SH06 · 공유 링크 · 로그인 필요 | [281:9261](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=281-9261) |
| SH07 · 이 정보를 볼 수 있는 참여 권한이 없어요 | [281:9297](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=281-9297) |
| SH08 · 지금은 이 정보를 열 수 없어요 | [281:9327](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=281-9327) |
| SH09 · 지금은 공유할 수 없어요 | [281:9357](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=281-9357) |
| DEL01 · 항목 삭제 확인 · 소유자 | [282:9281](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=282-9281) |
| DEL02 · 항목 삭제 확인 · 구성원 | [282:9339](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=282-9339) |
| DEL03 · 삭제를 요청했어요 | [282:9391](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=282-9391) |
| DEL04 · 항목 삭제 완료 | [282:9468](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=282-9468) |
| DEL05 · 삭제 후 일정 목록 | [282:9537](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=282-9537) |
| DEL06 · 유형 공통 삭제 결과 | [288:12079](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=288-12079) |
| DEL07 · 유형 공통 삭제 요청 | [288:12109](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=288-12109) |
| SRC01 · 소식에서 확인한 근거 | [283:9546](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=283-9546) |
| SRC02 · 직접 입력한 근거 | [283:9594](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=283-9594) |
| SRC03 · 답변을 확인한 근거 | [283:9640](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=283-9640) |
| SRC05 · 관찰의 근거 | [283:9686](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=283-9686) |
| SRC06 · 안내의 근거 | [283:9732](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=283-9732) |
| SRC07 · 질문이 생긴 근거 | [283:9778](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=283-9778) |
| SRC04 · 삭제된 근거 | [283:9824](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=283-9824) |
| HIS01 · 일정 변경 이력 | [284:9742](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=284-9742) |
| HIS02 · 담당자·상태 변경 이력 | [284:9792](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=284-9792) |
| HIS03 · 삭제된 항목의 이력 | [284:9847](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=284-9847) |
| HIS04 · 원본 삭제 후의 이력 | [284:9891](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=284-9891) |
| RC01 · 유효 근거 없음 · 재확인 필요 | [284:9928](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=284-9928) |
| RC02 · 재확인 · 새 내용 입력 | [284:10016](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=284-10016) |
| RC03 · 재확인 · 최종 확인 | [284:10079](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=284-10079) |
| RC04 · 재확인 후 상세 | [284:10116](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=284-10116) |
| RC05 · 재확인 필요 · 구성원 | [288:12177](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=288-12177) |
| L01 · 완료한 할 일 | [285:10171](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=285-10171) |
| L02 · 확인된 질문 | [285:10342](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=285-10342) |
| L03 · 할 일 첫 빈 화면 | [285:10488](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=285-10488) |
| L04 · 질문 첫 빈 화면 | [285:10654](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=285-10654) |
| L05 · 관찰 첫 빈 화면 | [285:10795](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=285-10795) |
| L06 · 안내 첫 빈 화면 | [285:10940](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=285-10940) |
| L07 · 남은 할 일 0건 | [285:11082](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=285-11082) |
| L08 · 첫 목록 불러오는 중 | [285:11246](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=285-11246) |
| L09 · 추가 목록 불러오기 실패 | [285:11399](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=285-11399) |
| EX01 · 다른 가족이 편집 중 | [286:11728](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=286-11728) |
| EX02 · 저장 전 최신 값 충돌 | [286:11833](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=286-11833) |
| EX03 · 선택한 담당자 참여 종료 | [286:11875](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=286-11875) |
| EX04 · 처리 실패가 확인된 상태 | [286:11907](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=286-11907) |
| EX05 · 접근 권한 또는 대상 없음 | [286:11942](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=286-11942) |
| EX06 · 오프라인 | [286:11963](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=286-11963) |
| EX07 · 처리 결과 확인 중 | [286:11993](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=286-11993) |
| EX08 · 작성 중 나가기 | [286:12025](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=286-12025) |
| EX09 · 변경 없음 | [286:12048](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=286-12048) |
| EX10 · 충돌 후 최신 값 재검토 | [286:12117](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=286-12117) |
| EX11 · 로그인 만료 | [286:12161](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=286-12161) |
| EX12 · 권한 변경 · 참여는 유지 | [286:12182](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=286-12182) |
| C40 · 입력 오류 · 작성값 유지 | [263:6546](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=263-6546) |
| C41 · 등록 요청 실패 · 확정 실패 | [263:6611](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=263-6611) |
| C42 · 등록 결과 확인 필요 | [263:6653](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=263-6653) |
| CR01 · 내 돌봄 요청 목록 | [288:11731](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=288-11731) |
| CR02 · 소유자의 돌봄 요청 목록 | [288:11836](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=288-11836) |
| CR03 · 돌봄 변경 요청 검토 | [288:11940](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=288-11940) |
| CR04 · 요청 미적용 결과 | [288:12011](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=288-12011) |

### 04 · 소식

| 화면·상태 이름 | 노드 |
| --- | --- |
| N01 · 소식 메인 | [230:2805](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=230-2805) |
| N02 · 소식 상세 · 제안 있음 | [230:2922](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=230-2922) |
| N03 · 소식 작성 | [230:3008](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=230-3008) |
| N04 · 소식 작성 · 입력 전 | [230:3036](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=230-3036) |
| N05 · 저장 완료 · 분석 중 | [231:3021](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=231-3021) |
| N06 · 분석 완료 · 새 제안 없음 | [231:3097](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=231-3097) |
| N07 · 분석 실패 · 원문 보존 | [231:3157](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=231-3157) |
| N08 · 소식 등록 실패 | [231:3229](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=231-3229) |
| N09 · 확인할 내용 · 검토 대기 | [233:3210](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=233-3210) |
| N10 · 승인 요청 목록 · 소유자 | [233:3297](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=233-3297) |
| N11 · 승인 요청 목록 · 구성원 | [233:3389](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=233-3389) |
| N12 · 검토 대기 · 빈 목록 | [233:3455](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=233-3455) |
| N13 · 제안 검토 · 구성원 | [234:3452](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=234-3452) |
| N14 · 제안 검토 · 소유자 | [234:3482](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=234-3482) |
| N15 · 소유자 · 반영 전 확인 | [234:3557](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=234-3557) |
| N16 · 요청 완료 · 승인 대기 | [234:3657](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=234-3657) |
| N17 · 반영 완료 | [234:3737](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=234-3737) |
| N18 · 반영된 제안·요청 목록 | [234:3810](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=234-3810) |
| N19 · 미적용 제안·요청 목록 | [234:3889](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=234-3889) |
| N20 · 이미 요청한 내용 조회 | [234:3964](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=234-3964) |
| N21 · 검토 중 현재 정보 변경 | [235:3904](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=235-3904) |
| N22 · 선택 묶음 반영 실패 | [235:3951](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=235-3951) |
| N23 · 구성원 · 요청 실패 | [235:4008](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=235-4008) |
| N24 · 제안 검토 · 선택 0건 | [235:4092](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=235-4092) |
| N25 · 소식 · 빈 목록 | [236:4099](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=236-4099) |
| N26 · 소식 목록 · 불러오기 실패 | [236:4171](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=236-4171) |
| NP01 · 일정 제안 수정 | [302:12388](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=302-12388) |
| NP02 · 수정한 제안 확인 · 구성원 | [302:12458](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=302-12458) |
| NP03 · 수정한 제안 확인 · 소유자 | [302:12528](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=302-12528) |
| NP04 · 수정한 제안 · 승인 대기 | [302:12598](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=302-12598) |
| NP05 · 수정한 제안 · 반영 완료 | [302:12676](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=302-12676) |
| NP06 · 수정 제안 반영 후 · 일정 상세 | [302:12747](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=302-12747) |
| NT01 · 할 일 제안 수정 | [303:12712](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=303-12712) |
| NT05 · 할 일 제안 확인 · 구성원 | [303:12907](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=303-12907) |
| NT02 · 질문 제안 수정 | [303:12780](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=303-12780) |
| NT06 · 질문 제안 확인 · 구성원 | [303:12997](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=303-12997) |
| NT03 · 관찰 제안 수정 | [303:12822](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=303-12822) |
| NT07 · 관찰 제안 확인 · 구성원 | [303:13081](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=303-13081) |
| NT04 · 안내 제안 수정 | [303:12864](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=303-12864) |
| NT08 · 안내 제안 확인 · 구성원 | [303:13165](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=303-13165) |
| NG01 · 유형 공통 · 요청 성공 | [310:14259](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=310-14259) |
| NG02 · 유형 공통 · 반영 성공 | [310:14325](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=310-14325) |
| NL01 · 연결 대상 선택 전 | [304:13016](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=304-13016) |
| NL02 · 연결 대상 선택됨 | [304:13066](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=304-13066) |
| NL03 · 선택한 대상 비교 | [304:13111](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=304-13111) |
| NL04 · 연결 대상 결정 보류 | [304:13159](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=304-13159) |
| NL05 · 연결 후보 없음 | [304:13237](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=304-13237) |
| NL06 · 후보 불러오기 실패 | [304:13269](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=304-13269) |
| NL07 · 대상 선택 후 검토 · 구성원 | [305:13196](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=305-13196) |
| NL08 · 대상 선택 후 검토 · 소유자 | [305:13263](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=305-13263) |
| ND01 · 근거 연결 검토 · 구성원 | [305:13330](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=305-13330) |
| ND02 · 근거 연결 검토 · 소유자 | [305:13379](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=305-13379) |
| ND03 · 근거 연결 요청 · 승인 대기 | [305:13428](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=305-13428) |
| ND04 · 근거 연결 완료 | [305:13497](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=305-13497) |
| ND05 · 연결 완료 · 근거 목록 | [305:13558](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=305-13558) |
| ND06 · 근거 연결 · 선택 대상 비교 | [307:13759](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=307-13759) |
| NS01 · 소식 원본 삭제 · 영향 확인 | [306:13455](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=306-13455) |
| NS02 · 연결 항목 없는 원본 삭제 | [306:13502](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=306-13502) |
| NS03 · 원본 삭제 완료 | [306:13542](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=306-13542) |
| NS04 · 독립된 유효 근거가 남음 | [306:13622](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=306-13622) |
| NS05 · 원본 삭제 후 소식 목록 | [306:13692](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=306-13692) |
| NS06 · 원본 삭제 · 실패 확인 | [307:13662](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=307-13662) |
| NS07 · 원본 삭제 · 결과 미확인 | [307:13685](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=307-13685) |
| NS08 · 원본 삭제 · 권한 변경 | [307:13717](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=307-13717) |
| NS09 · 이미 삭제된 원본 | [307:13738](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=307-13738) |
| NE01 · 선택 대상 유효하지 않음 | [309:13739](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=309-13739) |
| NE02 · 제안 원본 근거 삭제됨 | [309:13771](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=309-13771) |
| NE03 · 제안 수정 저장 실패 | [309:13803](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=309-13803) |
| NE04 · 제안 수정 중 나가기 | [309:13838](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=309-13838) |
| NE05 · 상충하는 근거 비교 | [309:13861](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=309-13861) |
| NE06 · 제안 입력 오류 | [309:13903](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=309-13903) |
| NI01 · 승인 요청 목록 · 빈 화면 | [310:13882](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=310-13882) |
| NI02 · 반영 목록 · 빈 화면 | [310:13957](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=310-13957) |
| NI03 · 미적용 목록 · 빈 화면 | [310:14032](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=310-14032) |
| NI04 · 소식 목록 · 첫 로딩 | [310:14107](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=310-14107) |
| NI05 · 추가 소식 조회 실패 | [310:14168](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=310-14168) |

### 05 · 가족

| 화면·상태 이름 | 노드 |
| --- | --- |
| F01 · 가족 메인 · 소유자 | [247:4287](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=247-4287) |
| F02 · 가족 메인 · 구성원 | [247:4415](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=247-4415) |
| F03 · 가족 메인 · 첫 가족 초대 | [247:4519](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=247-4519) |
| F04 · 가족 초대 · 링크 준비 | [248:4555](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=248-4555) |
| F05 · 초대 링크 복사 완료 | [248:4627](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=248-4627) |
| F06 · 보낸 초대 링크 만료 | [248:4708](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=248-4708) |
| F07 · 초대 링크 발급 실패 | [248:4778](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=248-4778) |
| F08 · 구성원 관리 · 소유자 | [249:4772](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=249-4772) |
| F09 · 구성원 내보내기 확인 | [249:4870](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=249-4870) |
| F10 · 내보내기 완료 · 갱신된 가족 | [249:4915](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=249-4915) |
| F11 · 대상이 이미 나간 경우 | [249:5025](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=249-5025) |
| F12 · 새 소유자 선택 | [250:5006](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=250-5006) |
| F13 · 소유권 이전 확인 | [250:5055](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=250-5055) |
| F14 · 소유권 이전 완료 | [250:5103](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=250-5103) |
| F15 · 이전할 가족 없음 | [250:5186](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=250-5186) |
| F16 · 공간 나가기 확인 | [251:5190](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=251-5190) |
| F17 · 소유자가 나가려는 경우 | [251:5236](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=251-5236) |
| F18 · 이전 대상의 참여 종료 | [251:5288](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=251-5288) |
| F19 · 소유권 이전 후 · 가족 | [251:5320](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=251-5320) |
| F20 · 공간 전체 삭제 확인 | [252:5349](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=252-5349) |
| F21 · 나가기 완료 · 참여 공간 선택 | [252:5402](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=252-5402) |
| F22 · 참여 중인 공간 없음 | [252:5458](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=252-5458) |
| F23 · 가족 목록 불러오기 실패 | [252:5500](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=252-5500) |
| F24 · 소유권 이전 실패 · 확정 실패 | [253:5487](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=253-5487) |
| F25 · 현재 공간 접근 종료 | [253:5530](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=253-5530) |
| F26 · 참여 공간 선택 · 평상시 | [254:5548](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=254-5548) |
| F26-A · 홈 상단 바 · 공간 메뉴 닫힘 | [375:15035](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=375-15035) |
| F26-B · 홈 상단 바 · 공간 메뉴 열림 | [375:15054](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=375-15054) |
| F26-C · 공간 목록 로딩 | [386:15294](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=386-15294) |
| F26-D · 공간 목록 조회 실패 | [386:15357](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=386-15357) |
| F26-E · 공간 전환 중 | [386:15432](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=386-15432) |
| F26-F · 공간 전환 실패 | [386:15468](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=386-15468) |

### 06 · 전체

| 화면·상태 이름 | 노드 |
| --- | --- |
| AM01 · 전체 · 소유자 | [320:14278](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=320-14278) |
| AM02 · 전체 · 구성원 | [320:14482](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=320-14482) |
| AC01 · 내 계정 | [320:14654](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=320-14654) |
| AC02 · 내 계정 · 선택된 공간 없음 | [320:14740](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=320-14740) |
| AC03 · 계정 정보 불러오는 중 | [320:14794](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=320-14794) |
| AC04 · 계정 정보를<br>불러오지 못했어요 | [320:14815](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=320-14815) |
| SP01 · 공간 정보 · 소유자 | [321:14657](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=321-14657) |
| SP02 · 공간 정보 · 구성원 | [321:14744](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=321-14744) |
| SP03 · 공간 정보 수정 · 소유자 | [321:14840](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=321-14840) |
| SP04 · 공간 정보 수정 · 구성원 | [321:14882](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=321-14882) |
| SP05 · 수정 내용 없음 | [321:14924](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=321-14924) |
| SP06 · 입력값 확인 | [321:14962](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=321-14962) |
| SP07 · 수정 중 나가기 | [321:15009](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=321-15009) |
| SP08 · 공간 정보 변경 확인 | [321:15041](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=321-15041) |
| SP09 · 공간 정보 수정 요청 확인 | [321:15087](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=321-15087) |
| SP10 · 공간 정보 수정 · 승인 대기 | [321:15133](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=321-15133) |
| SP11 · 공간 정보 직접 반영 완료 | [321:15212](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=321-15212) |
| SP12 · 공간 정보 · 변경 후 | [321:15280](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=321-15280) |
| SP13 · 공간 정보 변경 후 · 구성원 | [322:15519](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=322-15519) |
| RV01 · 공간 정보 요청 검토 · 소유자 | [322:15138](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=322-15138) |
| RV02 · 공간 정보 요청 · 최종 반영 확인 | [322:15227](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=322-15227) |
| RV03 · 공간 정보 요청 · 반영 결과 | [322:15272](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=322-15272) |
| RV04 · 공간 정보 요청 · 미적용 확인 | [322:15342](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=322-15342) |
| RV05 · 공간 정보 요청 · 미적용 결과 | [322:15386](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=322-15386) |
| RV06 · 이미 처리된 요청이에요 | [322:15464](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=322-15464) |
| SE01 · 공간 정보 · 편집 잠금 | [323:15482](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=323-15482) |
| SE02 · 검토 중 정보가<br>바뀌었어요 | [323:15516](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=323-15516) |
| SE03 · 최신 공간 정보와 재비교 | [323:15539](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=323-15539) |
| SE04 · 내 역할이 바뀌었어요 | [323:15585](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=323-15585) |
| SE05 · 변경을 반영하지 못했어요 | [323:15640](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=323-15640) |
| SE06 · 수정 요청을<br>보내지 못했어요 | [323:15663](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=323-15663) |
| SE07 · 처리 결과를<br>확인하지 못했어요 | [323:15686](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=323-15686) |
| SE08 · 공간 정보를<br>불러오지 못했어요 | [323:15707](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=323-15707) |
| SE09 · 공간 정보 작업 처리 중 | [323:15730](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=323-15730) |
| LO01 · 로그아웃 확인 | [323:15766](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=323-15766) |
| LO02 · 로그아웃 완료 · 로그인 | [323:15805](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=323-15805) |
| LO03 · 로그아웃 상태를<br>확인하지 못했어요 | [323:15849](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=323-15849) |
| LO04 · 인증 만료 · 다시 로그인 | [323:15884](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=323-15884) |
| WD01 · 회원 탈퇴 · 소유 공간 2개 | [324:15759](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=324-15759) |
| WD02 · 회원 탈퇴 · 일부 공간 정리 완료 | [324:15848](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=324-15848) |
| WD03 · 회원 탈퇴 · 최종 확인 | [324:15904](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=324-15904) |
| WD04 · 회원 탈퇴 완료 | [324:15961](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=324-15961) |
| WD05 · 회원 탈퇴 · 소유 공간 확인 중 | [324:15993](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=324-15993) |
| WD06 · 소유 중인 공간을<br>확인하지 못했어요 | [324:16030](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=324-16030) |
| WD07 · 먼저 정리할<br>공간이 있어요 | [324:16053](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=324-16053) |
| WD08 · 회원 탈퇴 처리 중 | [324:16076](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=324-16076) |
| WD09 · 회원 탈퇴를<br>완료하지 못했어요 | [324:16117](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=324-16117) |
| WD10 · 회원 탈퇴 결과 미확인 | [324:16140](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=324-16140) |
| WC01 · 탈퇴 정리 · 새 소유자 선택 | [325:15997](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=325-15997) |
| WC02 · 탈퇴 정리 · 소유권 이전 확인 | [325:16041](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=325-16041) |
| WC03 · 탈퇴 정리 · 이전 완료 | [325:16086](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=325-16086) |
| WC04 · 탈퇴 정리 · 공간 전체 삭제 확인 | [325:16122](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=325-16122) |
| WC05 · 탈퇴 정리 · 공간 삭제 완료 | [325:16176](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=325-16176) |
| WC06 · 공간 정리를<br>완료하지 못했어요 | [325:16210](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=325-16210) |
| WC07 · 공간 정리 결과를<br>확인하지 못했어요 | [325:16233](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=325-16233) |
| WC08 · 탈퇴 정리 · 이전할 가족 없음 | [325:16259](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=325-16259) |
| WC09 · 다른 가족을<br>선택해 주세요 | [327:16341](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=327-16341) |
| WC10 · 이 공간의<br>소유자가 바뀌었어요 | [327:16364](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=327-16364) |
| WN01 · 탈퇴 전 소식 · 공간 선택 | [326:16181](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=326-16181) |
| WN02 · 탈퇴 전 소식 · 참여 공간 없음 | [326:16233](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=326-16233) |
| CS01 · 인터넷 연결을<br>확인해 주세요 | [326:16265](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=326-16265) |
| CS02 · 이 공간에<br>접근할 수 없어요 | [326:16291](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=326-16291) |
| CS03 · 참여 공간을<br>불러오지 못했어요 | [326:16319](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=326-16319) |
| CS04 · 탈퇴 후 · 남은 기록의 작성자 | [326:16347](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=326-16347) |
| CS05 · 참여 공간 불러오는 중 | [326:16412](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=326-16412) |
| CS06 · 공간 관리 · 결과 미확인 | [331:16367](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=331-16367) |
| CS07 · 작업을<br>완료하지 못했어요 | [331:16402](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=331-16402) |

## 조회 데이터와 미정 주석

전체 원본 식별자·변수 별칭·텍스트 스타일·미정 주석 67개는 [figma-snapshot.json](sources/figma-snapshot.json)에 있다. 미정 주석은 승인된 정책이 아니다. 개발 시 해당 프레임의 주변 설명과 [DECISIONS.md](DECISIONS.md)를 함께 확인한다.

