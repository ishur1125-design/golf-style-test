# 나의 라운드 스타일 — 골프 심리테스트

18홀 문항으로 필드 위의 성향을 살펴보는 웹 심리테스트입니다.
빌드 도구·의존성 없이 `index.html` 하나로 동작합니다.

## 구성

```
golf-style-test/
├── index.html     # 앱 전체 (HTML + CSS + JS, 외부 의존성 없음)
├── og-image.png   # 카카오톡·트위터 링크 미리보기 이미지 (1200×630)
└── README.md
```

## 배포 주소

`https://ishur1125-design.github.io/golf-style-test/` 기준으로 이미 설정되어 있습니다.
레포 이름을 `golf-style-test`가 아닌 다른 이름으로 만들 경우에만 `index.html`의 세 군데를 맞춰주세요.

| 위치 | 줄 | 용도 |
|---|---|---|
| `<meta property="og:url">` | 17 | 카톡 미리보기 링크 |
| `<meta property="og:image">` | 18 | 카톡 미리보기 썸네일 |
| `var SITE = ...` | 126 | 결과 카드 이미지 하단에 찍히는 주소 |

> og:image는 **절대 주소**여야 합니다. 상대 경로(`og-image.png`)로 두면 카카오톡에서 썸네일이 뜨지 않습니다.

## GitHub Pages 배포

1. GitHub에서 새 레포 생성 (이름: `golf-style-test`, Public)
2. 이 폴더의 세 파일을 업로드 — 웹에서 **Add file → Upload files**로 드래그하면 됩니다
3. **Settings → Pages → Source**를 `Deploy from a branch`, 브랜치는 `main` / `/ (root)`로 지정
4. 1~2분 뒤 `https://<사용자명>.github.io/golf-style-test/` 에서 열립니다

터미널로 올린다면:

```bash
cd golf-style-test && git init && git add . && git commit -m "나의 라운드 스타일" && git branch -M main && git remote add origin https://github.com/ishur1125-design/golf-style-test.git && git push -u origin main
```

## 방문자 집계 (GoatCounter)

기본값은 **꺼짐**입니다. `index.html`의 `var GC_CODE = "";`가 비어 있으면 집계 스크립트를
아예 불러오지 않고, 고지 문구도 표시되지 않습니다.

켜는 법:

1. [goatcounter.com](https://www.goatcounter.com)에서 무료 가입 (개인·비상업 용도 무료)
2. 원하는 코드를 정하면 대시보드 주소가 `https://<코드>.goatcounter.com`으로 발급됩니다
3. `index.html`에서 `var GC_CODE = "";`의 따옴표 안에 그 **코드만** 넣습니다
   ```js
   var GC_CODE = "golfstyle";   // 대시보드가 golfstyle.goatcounter.com 인 경우
   ```
4. 재업로드하면 끝. 주소 전체나 `https://`를 넣으면 안 됩니다

### 집계되는 항목

| 경로 | 시점 |
|---|---|
| `/golf-style-test/` | 페이지 방문 (자동) |
| `e/start` | 표지에서 시작하기 |
| `e/teeoff` | 인적사항 입력 후 첫 홀 진입 |
| `e/hole-01` ~ `e/hole-18` | 각 홀 응답 완료 — 이탈 지점 파악용 |
| `e/complete` | 18홀 완주 |
| `e/type/HHH` 등 8종 | 판정된 유형 |
| `e/card-saved` / `e/text-copied` | 결과 공유 시도 |

`e/teeoff` 대비 `e/complete` 비율이 완주율이고, `e/hole-NN`이 급감하는 지점이 이탈 구간입니다.
`e/type/*` 8개를 합치면 유형별 실제 분포가 나옵니다.

### 개인정보

쿠키를 사용하지 않고 개인을 식별하지 않습니다. **닉네임·성별·나이대·구력·평균 타수·라운드 목적은
전송하지 않습니다.** 전송되는 건 위 표의 경로 문자열과 GoatCounter가 자체 수집하는
접속 국가·유입 경로·브라우저 종류뿐입니다. 집계를 켜면 표지 하단에 이 사실이 자동으로 고지됩니다.

이벤트가 많다고 느껴지면 `answer()` 안의 `track('hole-...')` 한 줄만 지우세요.
방문·완주·유형 집계는 그대로 유지됩니다.

## 카카오톡 미리보기가 갱신되지 않을 때

카카오는 링크 미리보기를 캐싱합니다. 주소를 바꾸거나 이미지를 교체한 뒤 반영이 안 되면
[카카오 디벨로퍼스 캐시 초기화](https://developers.kakao.com/tool/clear/og)에서 URL을 넣고 초기화하세요.

## 문항 설계

18홀 = 18문항입니다.

- **리커트 15문항** (5점 척도) — 5개 요인 × 3문항
- **상황형 3문항** (4지선다) — 5홀·12홀·17홀에 배치, 각각 멘탈/코스 이해/배려심 채점에 반영
- **9홀 종료 후 "그늘집"** — 전반 집계를 바탕으로 한 줄 코멘트

### 측정 요인

| 코드 | 이름 | 문항 수 |
|---|---|---|
| E | 사교성 | 3 |
| A | 배려심 | 4 |
| C | 준비성 | 3 |
| S | 멘탈 | 4 |
| O | 코스 이해 | 4 |

요인별 점수는 0~100으로 정규화합니다. 유형은 **준비성(C) × 멘탈(S) × 사교성(E)**의
높음/낮음 조합으로 8가지가 나오고, 배려심(A)과 코스 이해(O)는 배지로 붙습니다.

### 인적사항이 바꾸는 것

성별·나이대·구력·평균 타수·라운드 목적을 받습니다. **점수 계산에는 관여하지 않고**,
아래 네 군데의 문구를 실제로 교체합니다.

1. 분석 중 화면의 대조 문구
2. 결과 도입부 한 줄 ("구력 3~7년, 평균 90대의 코스 매니저입니다")
3. **다음 라운드 한 가지** — 평균 타수(100타 이상 / 90대 / 80대 이하) × 8유형 = 24가지 처방
4. **라운드 목적 코멘트** — 목적 5종 × 멘탈 상·하 = 10가지

닉네임은 결과 화면과 저장 카드에 표시됩니다. 모든 입력은 브라우저 밖으로 나가지 않습니다.

## 결과 카드 저장

`결과 카드 이미지 만들기`를 누르면 1080×1350 PNG를 canvas로 생성합니다.
데스크톱은 저장 버튼, 모바일은 이미지 길게 눌러 저장하는 방식 양쪽을 지원합니다.

## 문항 출처

리커트 문항은 퍼블릭 도메인 문항 풀인 **IPIP**(International Personality Item Pool)의
Goldberg's Big-Five Factor Markers를 골프 상황으로 각색했습니다. 상황형 3문항은 자체 제작입니다.

**오락 목적의 콘텐츠이며 심리학적 진단이 아닙니다.**
