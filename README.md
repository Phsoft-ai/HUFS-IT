# HUFS-IT

단일 HTML 파일을 중심으로 빠르게 제작하는 웹 프로토타입 프로젝트입니다.

## 프로젝트 구조

```text
.
├── AGENTS.md
├── README.md
├── index.html
└── assets/
    └── .gitkeep
```

## 실행 방법

`index.html`을 브라우저에서 열면 됩니다.

GitHub Pages로 배포하려면 저장소의 **Settings → Pages**에서 다음과 같이 설정하세요.

- Source: `Deploy from a branch`
- Branch: `main`
- Folder: `/(root)`

이후 `main` 브랜치에 Push하면 GitHub Pages가 사이트를 자동으로 배포합니다.

## 협업 규칙

- 실제 화면과 기능은 기본적으로 `index.html`에서 작업합니다.
- 이미지, 아이콘, 폰트 등 정적 파일은 `assets/`에 저장합니다.
- API 키, 비밀번호, 개인정보 등 비밀 정보는 저장소에 올리지 않습니다.
- 작업 전 최신 `main` 브랜치를 가져오고, 작업 후 변경 사항을 확인한 뒤 Push합니다.
- 같은 파일을 동시에 수정하지 않도록 작업자를 조율합니다.
