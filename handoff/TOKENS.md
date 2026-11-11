# 한마음 디자인 토큰 조회표

기준일: 2026-10-01 KST. 출처: [한마음 Figma](https://www.figma.com/design/TNlkTZrM8NIo0jSp6tRTwH?node-id=172-2)의 로컬 변수와 텍스트 스타일. **실제 변수 94개, 텍스트 스타일 12개**를 조회했다. 이 파일은 조회표이며 아직 FE에 토큰을 구현한 것이 아니다.

## 구현 기준

- 컬렉션은 Hanmaum · Primitives / Hanmaum · Color / Hanmaum · Layout이다. 현재 각 컬렉션은 모드 하나이며 다크 모드는 설계되어 있지 않다.
- 의미가 있는 컴포넌트에는 Color의 의미별 토큰을 사용하고, 같은 값이라는 이유로 모든 색을 브랜드 색 하나로 합치지 않는다.
- 별칭은 기본 색을 참조한다. 아래 표의 값은 현재 별칭을 해석한 값이며, 코드에서도 참조 관계를 보존하는 것을 권장한다.
- CSS 열은 Figma에 지정된 코드 표기다. 소셜 토큰의 이름 접두사는 다른 토큰과 다르므로 읽지 않고 일괄 재명명하지 않는다. 코드 명명 변경 시 Figma↔코드 대응을 문서화한다.
- 숫자 변수는 간격·반경·크기 등 px 단위 용도로 설계되었다. Figma 390px을 브라우저 고정 폭으로 강제하지 않는다.
- 타이포그래피는 별도의 텍스트 스타일이다. 폰트 변수 생성과 혼동하지 않는다. **Noto Sans KR**을 사용한다.

현재 주요 색은 청록 **#24746B**, 청록 눌림 **#1C5F58**, 청록 배경 **#E9F4F1**, 오류 **#C93625**, 오류 눌림 **#B83224**, 오류 배경 **#FFF1ED**다. 초기 대화의 #DC3E26을 최종 오류 토큰으로 되돌리지 않는다.

## Hanmaum · Primitives · 18개

모드: Value.

| 변수 | 현재 값 | 참조 변수 | CSS 코드 표기 |
| --- | --- | --- | --- |
| `teal/50` | `#E9F4F1` | — | `var(--hm-teal-50)` |
| `teal/700` | `#24746B` | — | `var(--hm-teal-700)` |
| `neutral/0` | `#FFFFFF` | — | `var(--hm-neutral-0)` |
| `neutral/25` | `#F7F8F6` | — | `var(--hm-neutral-25)` |
| `neutral/50` | `#F0F3F0` | — | `var(--hm-neutral-50)` |
| `neutral/100` | `#EDF0ED` | — | `var(--hm-neutral-100)` |
| `neutral/200` | `#DCE3DE` | — | `var(--hm-neutral-200)` |
| `neutral/600` | `#61716B` | — | `var(--hm-neutral-600)` |
| `neutral/900` | `#25332F` | — | `var(--hm-neutral-900)` |
| `red/50` | `#FFF1ED` | — | `var(--hm-red-50)` |
| `red/600` | `#C93625` | — | `var(--hm-red-600)` |
| `red/700` | `#B83224` | — | `var(--hm-red-700)` |
| `teal/800` | `#1C5F58` | — | `var(--hm-teal-800)` |
| `social/kakao/background` | `#FEE500` | — | `var(--social-kakao-background)` |
| `social/kakao/text` | `#171A23` | — | `var(--social-kakao-text)` |
| `social/google/background` | `#FCFCFD` | — | `var(--social-google-background)` |
| `social/google/text` | `#1C2230` | — | `var(--social-google-text)` |
| `social/google/border` | `#C6CEDA` | — | `var(--social-google-border)` |

## Hanmaum · Color · 42개

모드: Light.

| 변수 | 현재 값 | 참조 변수 | CSS 코드 표기 |
| --- | --- | --- | --- |
| `color/bg/page` | `#F7F8F6` | `neutral/25` | `var(--hm-color-bg-page)` |
| `color/bg/surface` | `#FFFFFF` | `neutral/0` | `var(--hm-color-bg-surface)` |
| `color/bg/subtle` | `#F0F3F0` | `neutral/50` | `var(--hm-color-bg-subtle)` |
| `color/bg/brand-subtle` | `#E9F4F1` | `teal/50` | `var(--hm-color-bg-brand-subtle)` |
| `color/bg/canvas` | `#EDF0ED` | `neutral/100` | `var(--hm-color-bg-canvas)` |
| `color/text/primary` | `#25332F` | `neutral/900` | `var(--hm-color-text-primary)` |
| `color/text/secondary` | `#61716B` | `neutral/600` | `var(--hm-color-text-secondary)` |
| `color/text/brand` | `#24746B` | `teal/700` | `var(--hm-color-text-brand)` |
| `color/text/inverse` | `#FFFFFF` | `neutral/0` | `var(--hm-color-text-inverse)` |
| `color/icon/primary` | `#25332F` | `neutral/900` | `var(--hm-color-icon-primary)` |
| `color/icon/secondary` | `#61716B` | `neutral/600` | `var(--hm-color-icon-secondary)` |
| `color/icon/brand` | `#24746B` | `teal/700` | `var(--hm-color-icon-brand)` |
| `color/icon/inverse` | `#FFFFFF` | `neutral/0` | `var(--hm-color-icon-inverse)` |
| `color/action/primary` | `#24746B` | `teal/700` | `var(--hm-color-action-primary)` |
| `color/border/default` | `#DCE3DE` | `neutral/200` | `var(--hm-color-border-default)` |
| `color/border/focus` | `#24746B` | `teal/700` | `var(--hm-color-border-focus)` |
| `color/divider/default` | `#DCE3DE` | `neutral/200` | `var(--hm-color-divider-default)` |
| `color/feedback/error/text` | `#C93625` | `red/600` | `var(--hm-color-feedback-error-text)` |
| `color/feedback/error/icon` | `#C93625` | `red/600` | `var(--hm-color-feedback-error-icon)` |
| `color/feedback/error/bg` | `#FFF1ED` | `red/50` | `var(--hm-color-feedback-error-bg)` |
| `color/feedback/error/border` | `#C93625` | `red/600` | `var(--hm-color-feedback-error-border)` |
| `color/action/destructive` | `#C93625` | `red/600` | `var(--hm-color-action-destructive)` |
| `color/action/destructive-pressed` | `#B83224` | `red/700` | `var(--hm-color-action-destructive-pressed)` |
| `color/action/primary-pressed` | `#1C5F58` | `teal/800` | `var(--hm-color-action-primary-pressed)` |
| `color/action/disabled` | `#DCE3DE` | `neutral/200` | `var(--hm-color-action-disabled)` |
| `color/text/disabled` | `#61716B` | `neutral/600` | `var(--hm-color-text-disabled)` |
| `color/icon/disabled` | `#61716B` | `neutral/600` | `var(--hm-color-icon-disabled)` |
| `color/border/disabled` | `#DCE3DE` | `neutral/200` | `var(--hm-color-border-disabled)` |
| `color/input/bg/default` | `#FFFFFF` | `neutral/0` | `var(--hm-color-input-bg-default)` |
| `color/input/bg/disabled` | `#F0F3F0` | `neutral/50` | `var(--hm-color-input-bg-disabled)` |
| `color/input/border/default` | `#DCE3DE` | `neutral/200` | `var(--hm-color-input-border-default)` |
| `color/input/border/focus` | `#24746B` | `teal/700` | `var(--hm-color-input-border-focus)` |
| `color/input/border/error` | `#C93625` | `red/600` | `var(--hm-color-input-border-error)` |
| `color/input/border/disabled` | `#DCE3DE` | `neutral/200` | `var(--hm-color-input-border-disabled)` |
| `color/input/text/placeholder` | `#61716B` | `neutral/600` | `var(--hm-color-input-text-placeholder)` |
| `color/input/text/disabled` | `#61716B` | `neutral/600` | `var(--hm-color-input-text-disabled)` |
| `color/border/interactive` | `#61716B` | `neutral/600` | `var(--hm-color-border-interactive)` |
| `color/social/kakao/background` | `#FEE500` | `social/kakao/background` | `var(--color-social-kakao-background)` |
| `color/social/kakao/text` | `#171A23` | `social/kakao/text` | `var(--color-social-kakao-text)` |
| `color/social/google/background` | `#FCFCFD` | `social/google/background` | `var(--color-social-google-background)` |
| `color/social/google/text` | `#1C2230` | `social/google/text` | `var(--color-social-google-text)` |
| `color/social/google/border` | `#C6CEDA` | `social/google/border` | `var(--color-social-google-border)` |

## Hanmaum · Layout · 34개

모드: Value.

| 변수 | 현재 값 | 참조 변수 | CSS 코드 표기 |
| --- | --- | --- | --- |
| `spacing/0` | `0` | — | `var(--hm-spacing-0)` |
| `spacing/2` | `2` | — | `var(--hm-spacing-2)` |
| `spacing/3` | `3` | — | `var(--hm-spacing-3)` |
| `spacing/4` | `4` | — | `var(--hm-spacing-4)` |
| `spacing/5` | `5` | — | `var(--hm-spacing-5)` |
| `spacing/6` | `6` | — | `var(--hm-spacing-6)` |
| `spacing/8` | `8` | — | `var(--hm-spacing-8)` |
| `spacing/10` | `10` | — | `var(--hm-spacing-10)` |
| `spacing/12` | `12` | — | `var(--hm-spacing-12)` |
| `spacing/14` | `14` | — | `var(--hm-spacing-14)` |
| `spacing/16` | `16` | — | `var(--hm-spacing-16)` |
| `spacing/20` | `20` | — | `var(--hm-spacing-20)` |
| `spacing/24` | `24` | — | `var(--hm-spacing-24)` |
| `spacing/28` | `28` | — | `var(--hm-spacing-28)` |
| `spacing/32` | `32` | — | `var(--hm-spacing-32)` |
| `spacing/40` | `40` | — | `var(--hm-spacing-40)` |
| `spacing/48` | `48` | — | `var(--hm-spacing-48)` |
| `radius/0` | `0` | — | `var(--hm-radius-0)` |
| `radius/5` | `5` | — | `var(--hm-radius-5)` |
| `radius/6` | `6` | — | `var(--hm-radius-6)` |
| `radius/8` | `8` | — | `var(--hm-radius-8)` |
| `radius/9` | `9` | — | `var(--hm-radius-9)` |
| `radius/10` | `10` | — | `var(--hm-radius-10)` |
| `radius/12` | `12` | — | `var(--hm-radius-12)` |
| `radius/14` | `14` | — | `var(--hm-radius-14)` |
| `radius/16` | `16` | — | `var(--hm-radius-16)` |
| `radius/18` | `18` | — | `var(--hm-radius-18)` |
| `radius/20` | `20` | — | `var(--hm-radius-20)` |
| `radius/24` | `24` | — | `var(--hm-radius-24)` |
| `radius/full` | `999` | — | `var(--hm-radius-full)` |
| `size/touch-min` | `44` | — | `var(--hm-size-touch-min)` |
| `size/control` | `52` | — | `var(--hm-size-control)` |
| `border/width/default` | `1` | — | `var(--hm-border-width-default)` |
| `border/width/focus` | `2` | — | `var(--hm-border-width-focus)` |

## 텍스트 스타일 · 12개

| 스타일 | 폰트 | 굵기 | 크기 | 행 높이 |
| --- | --- | --- | ---: | ---: |
| `Hanmaum/Title/Page` | Noto Sans KR | Bold (700) | 26px | 38px |
| `Hanmaum/Title/Hero` | Noto Sans KR | Bold (700) | 22px | 32px |
| `Hanmaum/Heading/Section` | Noto Sans KR | Bold (700) | 18px | 28px |
| `Hanmaum/Body/Default` | Noto Sans KR | Regular (400) | 16px | 26px |
| `Hanmaum/Body/Reading` | Noto Sans KR | Regular (400) | 16px | 28px |
| `Hanmaum/Body/Emphasis` | Noto Sans KR | Medium (500) | 16px | 26px |
| `Hanmaum/Body/Small` | Noto Sans KR | Regular (400) | 14px | 24px |
| `Hanmaum/Label/Button` | Noto Sans KR | Bold (700) | 16px | 24px |
| `Hanmaum/Caption/Default` | Noto Sans KR | Regular (400) | 13px | 22px |
| `Hanmaum/Label/Badge` | Noto Sans KR | Medium (500) | 12px | 18px |
| `Hanmaum/Navigation/Default` | Noto Sans KR | Regular (400) | 12px | 18px |
| `Hanmaum/Navigation/Selected` | Noto Sans KR | Bold (700) | 12px | 18px |

## 기본 규격과 검증 범위

- 일반 기본 버튼 높이 52px, 최소 터치 크기 44px 관련 변수가 있다.
- 소셜 로그인 버튼은 현재 컴포넌트 기준 350×54px, 모서리 10px, Noto Sans KR 16px Bold다. 작은 화면에서는 컨테이너에 맞게 반응형으로 적용한다.
- 컴포넌트에 존재하는 hover/pressed/focus/disabled/loading 등은 [컴포넌트 색인](FIGMA.md)을 본다.
- 전체 텍스트 대비·키보드·스크린리더·모바일 브라우저 검증을 이번 문서화에서 수행하지 않았다. 코드 구현 후 실제 상호작용 기준으로 검증한다.
- 원시 변수 ID·모드·CSS 표기·스타일 ID는 [Figma 조회 데이터](sources/figma-snapshot.json)에 보존했다.

