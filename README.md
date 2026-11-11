# 한마음 프로젝트 문서

한마음은 가족이 부모님의 돌봄 소식을 함께 기록하고 필요한 일정을 챙기는 웹앱입니다. AI가 소식을 분석해 일정·할 일·확인사항을 제안하면, 가족이 검토하고 공간 소유자가 반영을 확정합니다.

`hanmaum-docs`는 한마음의 프로젝트 기획서, 기능 명세서, 제품 정책서와 FE·BE·AI 공통 개발 문서를 관리하는 저장소입니다.

## 📁 폴더 구조

```text
hanmaum-docs/
├── README.md
├── product/                         # 프로젝트 기획서, 기능 명세서, 제품 정책서
│   └── .gitkeep
├── development/                     # 공통 아키텍처, 서비스 연동, 검증 문서
│   └── .gitkeep
├── reports/                         # 졸업작품 결과보고서 원본 및 제출용 PDF
│   └── .gitkeep
├── presentation/                    # 졸업작품 포스터 및 발표 자료
│   └── .gitkeep
└── assets/                          # 문서에서 함께 사용하는 그림과 도표
    └── architecture/
        ├── hanmaum-architecture.drawio
        ├── hanmaum-architecture.svg
        └── hanmaum-architecture.png
```

`.gitkeep`만 있는 폴더는 문서를 추가할 예정입니다.
