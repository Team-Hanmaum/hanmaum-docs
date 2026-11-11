# 기능 명세서 원문 조회본

조회일: 2026-10-01 KST. 원본: https://app.notion.com/p/3d10f25ea13880e28826fa4ebc3c428d?pvs=204

원본 수정 시각: 2026-09-27T09:31:58.195Z; Notion 검증 표기: unverified. 조회일과 원문 내 기준일을 구분한다.

Here is the result of "fetch" for the Page with URL https://app.notion.com/p/3d10f25ea13880e28826fa4ebc3c428d as of 2026-09-27T15:48:03.169Z:
<page url="https://app.notion.com/p/3d10f25ea13880e28826fa4ebc3c428d" icon="icons/hammer_yellow">
<ancestor-path>
<parent-page url="https://app.notion.com/p/3d10f25ea1388054b6facfcd0cdc12f3" title=""/>
<ancestor-2-page url="https://app.notion.com/p/3d10f25ea13880318827deac3a5e9ae0" title=""/>
<ancestor-3-page url="https://app.notion.com/p/3d10f25ea1388099b7c1c7729ff41404" title="졸작"/>
</ancestor-path>
<properties>
{"title":"기능 명세서"}
</properties>
<iconMetadata>{"type":"icon","icon":{"name":"hammer","color":"yellow"}}</iconMetadata>
<content>
<callout>
	## MVP 기능 분류 {toggle="true"}
		<callout>
			## MVP 기능 분류
			---
			<table header-row="true">
			<colgroup>
			<col width="193.3125">
			<col width="737.3125">
			</colgroup>
<tr>
<td>분류</td>
<td>내용</td>
</tr>
<tr>
<td><span color="orange">**Auth**</span>** (인증·계정)**</td>
<td>구글·카카오 로그인, 로그인 상태 유지, 로그아웃, 사용자 정보 조회, 회원 탈퇴·계정 정보 삭제</td>
</tr>
<tr>
<td><span color="blue">**Space**</span>** (돌봄 공간·가족)**</td>
<td>돌봄 공간 생성·조회·수정·삭제, 24시간 초대 링크, 가족 참여·나가기·내보내기, 소유권 이전, 소유자·구성원 권한</td>
</tr>
<tr>
<td><span color="pink">**Record**</span>** (소식 기록)**</td>
<td>자유로운 소식 입력, 원본 보존·조회·삭제, 원본 삭제 시 연결 정보 정리</td>
</tr>
<tr>
<td><span color="green">**AI**</span>** (분석·기존 정보 연결)**</td>
<td>소식 분류·정보 추출, 등록 시점 기준 날짜 해석, 기존 정보 연결, 추가·변경·해소 제안, 중복·상충 후보 식별</td>
</tr>
<tr>
<td><span color="purple">**Proposal**</span>** (제안 확인·반영)**</td>
<td>AI 제안·근거 비교, 구성원의 변경 요청, 소유자 최종 승인, 개별 선택·수정, 선택 제안 전체 검증 후 반영</td>
</tr>
<tr>
<td><span color="brown">**Item**</span>** (돌봄 항목 관리)**</td>
<td>관리 항목 등록·수정·삭제, 일정 완료·취소, 할 일 단일 담당자·3단계 상태, 확인사항 해소 승인, 항목별 편집 잠금</td>
</tr>
<tr>
<td><span color="yellow">**Dashboard**</span>** (현황판)**</td>
<td>예정·지난 일정, 남은 할 일, 미확인 확인사항, 최근 확정 변경 조회</td>
</tr>
<tr>
<td><span color="blue">**History**</span>** (근거·변경 이력)**</td>
<td>유효한 원본 근거와 변경 전후·행위자·시점 조회, 원본 삭제 시 민감 내용 제거</td>
</tr>
<tr>
<td><span color="red">**Share**</span>** (카카오톡 공유)**</td>
<td>최소 정보 카드 공유, 로그인·공간 참여 확인 후 상세·답변, 비구성원 참여 안내</td>
</tr>
<tr>
<td><span color="gray">**Notification**</span>** (알림)**</td>
<td>서비스 내부 알림, 본인 담당 할 일 기본 수신, 개인 설정·읽음·중복 방지</td>
</tr>
<tr>
<td><span color="gray">**Common**</span>** (공통·접근 환경)**</td>
<td>모바일 홈 화면 설치와 서비스 진입 지원 (Could)</td>
</tr>
			</table>
			- **MoSCoW (모스크바, 기능 우선순위)**:
				- <span color="red">**Must Have**</span>**: **프로젝트의 본질적 목표 달성과 정상 구동을 위해 무조건 구현해야 하는 **핵심 필수 기능**
				- <span color="yellow">**Should Have**</span>: 핵심 동작에는 지장이 없지만, 서비스의 완성도와 사용성을 위해 **가급적 구현해야 하는** **권장 기능**
				- <span color="green">**Could Have**</span>: 시간과 개발 리소스에 여유가 있을 때 추가하면 좋은 **부가적인(Nice-to-have) 기능**
				- <span color="gray">**Won’t Have**</span>: 이번 MVP에서 **구현 범위에 포함하지 않는 기능**
		</callout>
	## 기능 명세서 개정 이력 {toggle="true"}
		<database url="https://app.notion.com/p/f810f25ea138823fbac581899c6441c5" inline="true" data-source-url="collection://7060f25e-a138-83c0-85a3-07c069df4e5f">한마음 개정 이력</database>
</callout>
# 한마음 기능 명세서
<callout color="purple_bg">
	📌 **현재는 확정한 MVP 정책을 반영한 한마음 기능 목록·요약 단계입니다.**
	- 작성 범위: **Auth · Space · Record · AI · Proposal · Item · Dashboard · History · Share · Notification · Common** 11개 분류, 총 **100개 기능**
	- 각 항목에 **기능 ID · MoSCoW · 기능명 · 기능 설명**을 작성했습니다. 우선순위별 탭에서 **Must 67개 · Should 15개 · Could 9개 · Won't 9개**를 확인할 수 있습니다.
	- 상세 요구사항은 추후 각 기능 페이지에 **기능 개요 · 기능 설명 · 기능 요구사항(입력 / 출력) · 정상 흐름 · 예외 흐름 · 제약 사항 · 비고** 형식으로 작성합니다.
	- **소유자 최종 승인 · 항목별 편집 잠금 · 상태 전환 · 원본과 연결 정보 삭제** 등 확정한 기준을 기능 설명에 반영했습니다. 상세 제품 정책서는 별도로 작성합니다.
</callout>
<database url="https://app.notion.com/p/0eb0f25ea13882c78d5d01010159d572" inline="true" data-source-url="collection://80c0f25e-a138-828b-aa3a-0766e7c5d6e6">한마음 기능 명세서</database>
<empty-block/>
</content>
</page>
