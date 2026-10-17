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
