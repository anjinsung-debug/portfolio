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
| 활동 목록 제목 / 대표 활동 페이지 | `site/content/projects/_index.md` / `site/content/projects/apple.md` |
| 홈 아바타 이미지 | `site/assets/images/avatar.png` (테마가 150x150으로 줄여 출력) |
| 파비콘 | `site/static/` 의 `favicon*`, `apple-touch-icon.png`, `safari-pinned-tab.svg` |

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
