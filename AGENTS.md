# AGENTS.md

## 1. Project Architecture & Constraints (핵심 제약사항)
- **Single-File Prototype**: 모든 마크업(HTML), 스타일(CSS), 인터랙션(JS)은 오직 `index.html` 파일 하나에 작성한다. 외부 `.css`, `.js` 파일을 추가로 생성하거나 분리하지 않는다.
- **No Backend / No Build Step**: 순수 정적 프로토타입이다. 백엔드 API 호출, DB 연결, 번들러(Vite, Webpack 등), `package.json` 생성을 엄격히 금지한다.
- **Directory Structure**:
  ```text
  html-prototype-template/
  ├── README.md
  ├── AGENTS.md
  ├── index.html       # 유일한 엔트리포인트 및 작업 파일
  └── assets/          # 이미지, 아이콘 등 정적 리소스만 보관
  ```

## 2. Workflow & Agent Role (작업 워크플로우)

1. **Input**: 사용자가 와이어프레임 이미지 또는 기초 변환된 HTML/CSS 스니펫을 제공한다.
2. **Process (알잘딱 고도화)**:
   - 제공된 초안을 분석해 모던하고 깔끔한 완성형 UI로 리팩토링하여 `index.html`에 통합한다.
   - 다중 화면이나 역할 전환(예: 학생 뷰 ↔ 교수자 뷰, 탭 이동, 모달 등)이 필요한 경우, 페이지 이동(`href="other.html"`) 없이 단일 파일 내 JavaScript 상태(State) 변경 및 DOM 제어(`display: none/block` 또는 동적 렌더링)로 매끄럽게 전환되게 구현한다.
   - 빈 데이터 영역에는 실제 서비스처럼 보이는 자연스러운 한국어 더미 데이터(Mock Data)와 인터랙션(버튼 클릭 시 토스트 메시지, 탭 필터링, 오답률 차트 애니메이션 등)을 알아서 채워 넣는다.
3. **Deployment**: 작업 완료 또는 사용자의 요청 시 Git에 커밋 및 푸시하여 GitHub Pages(`github.io`)로 즉시 반영되게 한다.

## 3. UI/UX & Styling Rules (스타일 가이드)

- **Font (고정)**: 모든 텍스트는 Pretendard(프리텐다드)를 기본 폰트로 강제한다. `<head>`에 아래 CDN을 항상 포함하고 전역 적용한다.
  ```html
  <link rel="stylesheet" as="style" crossorigin href="https://cdn.jsdelivr.net/gh/orioncactus/pretendard@v1.3.9/dist/web/static/pretendard.min.css" />
  ```
  ```css
  * {
    font-family: "Pretendard Variable", Pretendard, -apple-system, BlinkMacSystemFont, system-ui, Roboto, sans-serif;
    box-sizing: border-box;
  }
  ```
- **Styling Engine**: 빠른 프로토타이핑을 위해 Tailwind CSS CDN(`<script src="https://cdn.tailwindcss.com"></script>`)과 커스텀 `<style>` 태그를 혼용한다.
- **Icons**: 외부 이미지가 없으면 Lucide Icons(`https://unpkg.com/lucide@latest`) 또는 인라인 SVG를 사용해 깨지는 아이콘이 없도록 한다.
- **Design Aesthetic**:
  - 여백이 충분하고 정보 위계가 명확한 미니멀·클린 UI(토스 스타일의 깔끔한 카드 레이아웃, 부드러운 그림자, `border-radius: 12px~16px`, 명확한 타이포그래피 대비)를 기본값으로 삼는다.
  - 클릭 가능한 모든 요소(`button`, 탭, 카드 등)에는 `cursor: pointer`와 부드러운 `hover` / `active` 트랜지션을 반드시 부여한다.
- **Snappy & Bouncy Motion (쫀득한 인터랙션 가이드)**:
  - 밋밋한 기본 `ease` 대신 탄성감이 느껴지는 Spring 타이밍 함수를 기본으로 사용한다:
    - 감속 스내피 커브: `cubic-bezier(0.16, 1, 0.3, 1)`
    - 쫀득한 탄성/오버슈트 커브: `cubic-bezier(0.34, 1.56, 0.64, 1)`
  - **버튼/인터랙티브 요소 피직스**:
    - `hover`: 살짝 떠오르며 미세 확대 (`transform: translateY(-2px) scale(1.01)`)
    - `active (클릭 시)`: 고무공처럼 쫀득하게 압축 (`transform: scale(0.94~0.96)`)
    - 트랜지션: `transition: all 0.2s cubic-bezier(0.34, 1.56, 0.64, 1)`
  - **모달·토스트·화면 전환 모션**: 단순 fade-in 대신 살짝 작아진 상태에서 탄성 있게 튕기며 등장하는 `spring-pop` 애니메이션을 적용한다.

## 4. Coding & Asset Rules (구현 및 경로 규칙)

- **Relative Paths Only (중요)**: GitHub Pages(`https://<username>.github.io/<repo-name>/`) 배포 시 루트 경로(`/assets/...`)를 쓰면 404 에러가 발생한다. 모든 에셋 경로는 반드시 상대 경로(`./assets/...`)로 작성한다.
- **Self-Contained State**: 새로고침 전까지 사용자의 입력(문제 생성, 필터 정렬, 북마크 토글 등)이 화면에서 실제로 동작하는 것처럼 보이도록 바닐라 JS 배열/객체 기반의 인메모리(In-memory) 상태 관리를 구현한다.
- **Partial Edit Caution**: `index.html`이 길어지더라도 기존 코드의 레이아웃이나 스크립트 변수가 깨지지 않도록 전체 컨텍스트를 유지하며 수정한다.

## 5. Git & Deployment Commands (배포 규칙)

- 배포 요청 시 별도의 빌드 과정 없이 아래 순서로 즉시 푸시한다:
  1. `git status`로 변경 파일 확인 (`index.html`, `assets/` 외 불필요한 임시 파일 제외)
  2. `git add index.html assets/`
  3. `git commit -m "feat: update prototype UI"` (작업 내용에 맞게 커밋 메시지 작성)
  4. `git push origin main` (또는 현재 체크아웃된 기본 브랜치)
