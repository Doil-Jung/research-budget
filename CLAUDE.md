# 연구지도 예산관리

연구지도(전람회·RnE·스팀클럽) 팀별 예산과 지출을 관리하는 교사용 웹 앱이다. 서버 없이 `index.html` 한 파일로 동작한다.

## 구조

- `index.html`: HTML, CSS, JS가 모두 들어 있다. 화면은 대시보드, 지출 내역, 품의서 업로드, 정산 보고서, 설정 5개다(`data-view`, `#view-*`).
- 외부 라이브러리는 CDN으로만 불러온다: Noto Sans KR(Google Fonts), PDF.js 3.4.120(cdnjs).
- 빌드 단계와 패키지가 없다. 브라우저에서 파일을 바로 열어 확인한다.

## 데이터

- 브라우저 `localStorage`의 `researchBudgetApp` 키 하나에 `teams`, `expenses`, `uploadedFiles`, `categories`를 저장한다(`loadState`, `saveState`).
- 실제 사용 데이터는 사용자 브라우저에만 있다. 저장소에는 없다. 설정 화면의 내보내기/가져오기(JSON)로 옮긴다.
- 저장 형식을 바꿀 때는 `loadState`의 기존 데이터 이전(migration) 방식처럼 예전 데이터가 깨지지 않게 한다.
- 지출 반영 금액은 실지출액이 있으면 실지출액, 없으면 품의액이다(`getReflected`).
- 기본 예산은 `createDefaultBudgets`에 있다. 전람회 식비는 `100000 - 10000 * 인원수`다.

## 품의서 PDF 읽기

- 업로드한 지출품의서 PDF에서 PDF.js로 글자를 뽑아 날짜, 금액, 품목, 팀, 항목을 추정한다(`parsePdfFile`, `extract*`, `matchTeam`, `matchCategory`).
- 팀은 파일 이름(예: `014 스팀클럽1.pdf`)을 먼저 보고, 없으면 본문에서 팀원 이름을 찾는다.
- 원본 품의서 PDF는 학교 내부 문서라서 저장소에 올리지 않는다(`.gitignore`). 실제 파일로 시험해야 하면 사용자에게 로컬에서 확인해 달라고 한다.

## 작업 환경

- 로컬 원본은 원드라이브 `코딩 작업 폴더/연구지도 예산관리`에 있다. 실제 git 저장소는 학교 컴퓨터(DESKTOP-2F9E1LF)의 `C:\git-repos\research_budget.git`이고 폴더 안 `.git`은 포인터 파일이다.
- 노트북(DOILRNE)에는 이 저장소가 없다. 클라우드에서 고친 내용은 학교 컴퓨터에서 `git pull`로 받는다.
- index.html의 기본 팀 데이터에 학생 이름이 들어 있으므로 저장소는 비공개로 유지한다.
