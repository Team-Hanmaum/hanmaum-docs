# 한마음 프로젝트 문서

한마음은 가족이 부모님의 돌봄 소식을 함께 기록하고 필요한 일정을 챙기는 웹앱입니다. AI가 소식을 분석해 일정·할 일·확인사항을 제안하면, 가족이 검토하고 공간 소유자가 반영을 확정합니다.

`hanmaum-docs`는 한마음의 프로젝트 기획서, 기능 명세서, 제품 정책서와 FE·BE·AI 공통 개발 문서를 관리하는 저장소입니다.

## 문서 바로가기

### 제품 문서

- [프로젝트 기획서](product/project-plan.md)
- [기능 명세서](product/feature-spec.md)
- [제품 정책서](product/product-policy.md)
- [정보 구조도(IA)](product/ia.md)
- [유저 플로우](product/user-flow.md)

### 공통 개발 문서

- [시스템 아키텍처](development/architecture.md)
- [CI/CD](development/ci-cd.md)
- [개발 워크플로우](development/workflow.md)
- [API 안내](development/api.md)

## 주요 다이어그램

### 정보 구조도(IA)

![한마음의 서비스 진입 화면과 돌봄 공간의 다섯 탭을 보여 주는 정보 구조도](assets/information-architecture/Information%20Architecture.png)

### 시스템 아키텍처

![Vercel 프론트엔드와 AWS Lightsail의 백엔드·AI·데이터베이스 구성](assets/development/system-architecture.png)

### CI/CD

![GitHub Actions에서 프론트엔드를 Vercel에, 백엔드와 AI를 Lightsail에 배포하는 흐름](assets/development/ci-cd.png)

### 유저 플로우 4장

**01. 첫 진입·돌봄 공간 만들기**

![로그인 상태와 참여 공간 수에 따라 돌봄 공간 홈에 진입하는 흐름](assets/userflow/01-userflow.png)

**02. 가족 초대·참여**

![초대 링크를 보내고 받은 가족이 돌봄 공간에 참여하는 흐름](assets/userflow/02-userflow.png)

**03. 소식 남기기·AI 분석**

![소식 원문을 등록하고 AI 분석 결과를 확인하는 흐름](assets/userflow/03-userflow.png)

**04. 제안 검토·반영**

![AI 제안을 검토하고 구성원과 소유자의 역할에 따라 반영하는 흐름](assets/userflow/04-userflow.png)

## 📁 폴더 구조

```text
hanmaum-docs/
├── README.md
├── product/                         # 프로젝트 기획서, 기능 명세서, 제품 정책서, IA, 유저 플로우
│   ├── project-plan.md
│   ├── feature-spec.md
│   ├── product-policy.md
│   ├── ia.md
│   └── user-flow.md
├── development/                     # 시스템 아키텍처, CI/CD, 개발 워크플로우, API 안내
│   ├── architecture.md
│   ├── ci-cd.md
│   ├── workflow.md
│   └── api.md
├── reports/                         # 졸업작품 결과보고서 원본 및 제출용 PDF
│   └── .gitkeep
├── presentation/                    # 졸업작품 포스터 및 발표 자료
│   └── .gitkeep
└── assets/                          # 문서에서 함께 사용하는 그림과 도표
    ├── development/
    │   ├── ci-cd.png
    │   └── system-architecture.png
    ├── information-architecture/
    │   └── Information Architecture.png
    └── userflow/
        ├── 01-userflow.png
        ├── 02-userflow.png
        ├── 03-userflow.png
        └── 04-userflow.png
```
