# 안진성 포트폴리오

Hugo와 PaperMod 테마로 만든 개인 소개 사이트입니다.

- 사이트: <https://anjinsung-debug.github.io/portfolio/>
- 배포: `main` 브랜치에 push하면 GitHub Actions가 빌드해 GitHub Pages에 올립니다.

## 필요한 도구와 버전

| 도구 | 버전 | 용도 |
|---|---|---|
| Hugo **extended** | 0.167.0 | 사이트 빌드·로컬 미리보기 (Actions도 같은 버전 사용) |
| Python | 3.9 이상 (작성 환경 3.14.3) | 배포와 같은 방식의 빌드(`.github/pages_build.py`) |
| Git | 작성 환경 2.55.0 | 저장소 복제·커밋·push |

npm·Node.js 의존성은 없습니다.

## 설치

Windows(winget) 기준입니다. 다른 OS는 [Hugo 릴리스 페이지](https://github.com/gohugoio/hugo/releases/tag/v0.167.0)에서
`hugo_extended_0.167.0_*` 파일을 받습니다.

```bash
winget install --id Hugo.Hugo.Extended --version 0.167.0 --exact
hugo version
git clone https://github.com/anjinsung-debug/portfolio.git
cd portfolio
```

`hugo version` 결과에 `v0.167.0`과 `+extended`가 보이면 됩니다. 설치 직후에는 터미널을 새로 열어야 `hugo` 명령을 찾습니다.

## 로컬에서 확인하기

### 미리보기 서버

```bash
cd site
hugo server --config hugo.toml,pages.json
```

브라우저에서 <http://localhost:1313/portfolio/>를 엽니다. 파일을 저장하면 화면이 자동으로 새로고침됩니다.
설정 파일은 반드시 `hugo.toml,pages.json` 두 개를 함께 지정해야 배포와 같은 설정으로 보입니다.

### 배포와 같은 빌드

저장소 최상위 폴더에서 실행합니다.

```bash
python .github/pages_build.py --build
```

결과는 `public/`에 생성되며 `public/index.html`이 홈 페이지입니다. Actions의 빌드 단계도 이 명령을 실행합니다.
`public/`과 `site/public/`은 `.gitignore`에 있어 커밋되지 않습니다.

## 수정할 파일 위치

모든 사이트 작업은 `site/` 폴더에서 합니다.

| 바꾸려는 것 | 파일 |
|---|---|
| 사이트 제목·언어, 홈 이름·소개·아바타, 메뉴, GitHub 링크 | `site/hugo.toml` |
| 소개 페이지 | `site/content/about.md` |
| 활동 목록 제목 / 대표 활동 페이지 | `site/content/projects/_index.md` / `site/content/projects/apple/index.md` |
| 두 번째 활동(캡스톤) 페이지 | `site/content/projects/capstone/index.md` |
| 활동 페이지의 실행 화면 이미지 | 각 활동 폴더(`apple/`, `capstone/`)의 `*.png`. 본문에서 `{{< figure src="파일명.png" alt="…" caption="…" >}}`로 넣음 |
| 홈 아바타 이미지 | `site/assets/images/avatar.png` (테마가 150x150으로 줄여 출력) |
| 파비콘 | `site/static/` 의 `favicon*`, `apple-touch-icon.png`, `safari-pinned-tab.svg` |
| 키보드 초점 표시 등 추가 스타일 | `site/assets/css/extended/focus.css` (테마가 `css/extended/*.css`를 자동으로 합침) |
| 본문 표 안의 긴 경로·URL 줄바꿈 스타일 | `site/assets/css/extended/content.css` |
| 테마 템플릿 수정본 | `site/layouts/` (아래 표) |

테마 원본을 직접 고치지 않고, 바꿔야 하는 템플릿만 `site/layouts/`의 같은 경로로 복사해 수정했습니다.
Hugo는 같은 경로라면 `site/layouts/`의 파일을 테마보다 먼저 사용합니다.

| 수정본 | 원본 대비 바꾼 내용 |
|---|---|
| `site/layouts/_partials/header.html` | 테마 전환 버튼 이름 `Toggle theme` → `라이트/다크 테마 전환` |
| `site/layouts/_partials/footer.html` | 맨 위로 버튼 이름 `go to top` → `페이지 맨 위로` |
| `site/layouts/list.html` | 목록 링크 이름 `post link to …` → `… 페이지로 이동` |
| `site/layouts/404.html` | 숫자 404만 있던 화면 → 한국어 제목·안내·홈/소개/활동 링크 |
| `site/layouts/_partials/head.html` | 404 페이지의 문서 제목을 `페이지를 찾을 수 없음`으로 |
| `site/layouts/baseof.html`, `site/layouts/rss.xml`, `site/layouts/_partials/templates/opengraph.html` | Hugo 0.158 사용 중단 API(`LanguageDirection`, `LanguageCode`)를 `Direction`, `Locale`로 교체 |

건드리지 않는 파일:

- `site/themes/template/` — PaperMod 테마 원본 복사본입니다. 템플릿을 바꿔야 하면 같은 경로로 `site/layouts/`에 복사해서 고칩니다.
- `site/pages.json`, `.github-pages.json`, `.github/` — 수업 제공 스크립트(`prepare_github_pages.py`)가 만든 배포 설정입니다.
  `.github-pages.json`의 `"config": "hugo.toml,pages.json"`은 배포 빌드가 `site/hugo.toml`을 읽게 하는 값이므로 지우지 않습니다.
- 최상위의 `assets/`, `layouts/`, `i18n/`, `theme.toml` — 내려받은 PaperMod ZIP 원본입니다. 빌드에는 쓰이지 않습니다.

링크 주소를 쓸 때는 `/about/`처럼 `/`로 시작하지 않습니다. 이 사이트는 `/portfolio/` 하위 경로에 있어서
`/`로 시작하면 도메인 루트로 이동합니다. 메뉴·버튼은 `about/`처럼 상대 경로로, 본문의 페이지 간 링크는
`{{< ref "projects/apple" >}}` 단축 코드로 씁니다.

## 수정 후 재배포와 확인

1. 위 파일을 고치고 로컬 미리보기로 확인합니다.
2. 커밋하고 `main`에 push합니다.
   ```bash
   git add site/content/about.md
   git commit -m "소개 페이지 문장 수정"
   git push origin main
   ```
3. 저장소의 [Actions 탭](https://github.com/anjinsung-debug/portfolio/actions)에서 방금 커밋의
   **Deploy Hugo to GitHub Pages** 실행이 `build`·`deploy` 모두 초록색(성공)인지 확인합니다. 보통 1분 안에 끝납니다.
4. <https://anjinsung-debug.github.io/portfolio/>를 열어 바뀐 내용을 확인합니다.
   이전 화면이 보이면 브라우저 캐시이므로 `Ctrl+F5`로 새로고침합니다.

실행이 실패하면 실패한 단계의 로그를 열어 원인을 확인하고, 로컬에서 `python .github/pages_build.py --build`로
같은 오류가 나는지 먼저 재현합니다.

## 출처와 라이선스

### 외부 자료

| 자료 | 사용 위치 | 출처 | 이용 조건 |
|---|---|---|---|
| PaperMod 테마 | `site/themes/template/` (최상위는 ZIP 원본) | <https://github.com/adityatelange/hugo-PaperMod> (Download ZIP, 2026-10-17) | MIT License. 고지는 `LICENSE`, `site/themes/template/LICENSE`에 유지. 원본 설명서는 `THEME-README.md` |
| Hugo | 빌드 도구 (저장소에 포함하지 않음) | <https://github.com/gohugoio/hugo> | Apache License 2.0 |
| GitHub Pages 배포 준비 스크립트 | `prepare_github_pages.py`, `.github/pages_build.py`, `.github/workflows/hugo-pages.yml`, `site/pages.json`, `.github-pages.json` | 초급프로젝트 수업 제공 (버전 3.1) | 수업 배포 자료 |

사이트 하단의 "Powered by Hugo & PaperMod" 고지는 테마 출처 표시로 그대로 둡니다.

### 직접 제작 · AI 생성 구분

| 자료 | 위치 | 구분 |
|---|---|---|
| 소개·활동 글의 내용(관심 분야와 이유, 수행 시기, 역할, 배운 점 등) | `site/content/` | 본인 |
| 소개·활동 글의 문장 구성과 사과랑 코드 근거 정리 | `site/content/` | AI(Claude) 초안을 본인이 확인·수정 |
| 아바타 이미지 | `site/assets/images/avatar.png` | 본인 제공 미모티콘 이미지 |
| 사과랑·캡스톤 실행 화면 캡처 5장 | `site/content/projects/apple/`, `site/content/projects/capstone/` | 본인 캡처 (폭 1280px로 축소, 사과랑 화면의 문의 전화번호는 가림. 캡스톤 화면은 팀 공동 프론트엔드) |
| 파비콘 4종 (`favicon.ico`, `favicon-16x16.png`, `favicon-32x32.png`, `apple-touch-icon.png`) | `site/static/` | 본인 아바타(미모티콘)의 얼굴 부분을 잘라 줄인 이미지. 변환 스크립트는 AI(Claude) 작성 |
| `safari-pinned-tab.svg` | `site/static/` | AI 생성 단색 원 도형 (Safari 고정 탭 아이콘은 단색 벡터만 가능해 아바타를 쓰지 않음) |
| 사이트 설정 `site/hugo.toml`, 이 README | 저장소 | AI(Claude) 도움으로 작성, 본인이 실행·배포 결과로 확인 |

AI 사용 도구: Claude Code 데스크톱 앱(Claude Opus 5.5). 각 커밋 메시지의 `Co-Authored-By` 줄로 AI가 함께 작업한 커밋을 구분할 수 있습니다.
