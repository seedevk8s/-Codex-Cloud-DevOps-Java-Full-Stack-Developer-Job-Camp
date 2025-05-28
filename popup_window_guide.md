# 팝업 창과 CSS 레이아웃 완전 가이드

## 📋 개요

이 문서는 JavaScript `window.open()`으로 생성된 팝업 창과 CSS Flexbox를 이용한 중앙 정렬에 대한 상세 설명입니다.

---

## 🔧 코드 구조

### 1. 메인 페이지 (button-2.html)
```html
<form>
  <input type="button" value="공지 창 열기" 
         onclick="window.open('notice.html','_blank','width=300, height=400')">
</form>
```

### 2. 팝업 창 (notice.html)
```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>공지사항</title>
  <link rel="stylesheet" href="css/notice.css">
</head>
<body>
  <div id="container">
    <h1>공지사항</h1>
    <p>텍스트 1</p>
    <p>텍스트 2</p>
  </div>
</body>
</html>
```

### 3. CSS 스타일 (notice.css)
```css
* {
  box-sizing: border-box;
}

body {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 0;
  min-height: 100vh;
}

#container {
  width: 250px;
  height: 375px;
  padding: 100px 50px;
  border: 1px solid black;
  background: url('../images/notice.jpg') no-repeat left top;
  background-size: auto;
}
```

---

## 🖼️ 레이아웃 구조

### 새창 크기와 구성 요소

```
새창 전체 (300px × 400px)
├── 브라우저 프레임 (제목 표시줄, 테두리)
└── 뷰포트 (실제 콘텐츠 영역, 약 300px × 370px)
    └── Body (100vh = 뷰포트 전체 높이)
        └── #container (250px × 375px, 중앙 배치)
```

### 크기 관계
- **새창**: 300px × 400px
- **컨테이너**: 250px × 375px
- **여백**: 새창이 컨테이너보다 가로 50px, 세로 25px 더 큼

---

## 🎯 중앙 정렬의 두 가지 의미

### ❌ 새창 자체의 위치 (화면상)
- `window.open()`에서 `left`, `top` 속성을 지정하지 않음
- 브라우저가 기본값으로 위치 결정
- **결과**: 화면 중앙이 아닌 곳에 생성됨

### ✅ 새창 내부에서 컨테이너 위치
- CSS Flexbox로 제어
- **결과**: 새창 내부에서 완벽한 중앙 배치

---

## 🔍 CSS 상세 분석

### Universal Selector
```css
* {
  box-sizing: border-box;
}
```
- 모든 요소의 크기 계산에 padding과 border 포함

### Body (Flexbox Container)
```css
body {
  display: flex;           /* 플렉스 컨테이너 */
  justify-content: center; /* 수평 중앙 정렬 */
  align-items: center;     /* 수직 중앙 정렬 */
  margin: 0;               /* 기본 마진 제거 */
  min-height: 100vh;       /* 뷰포트 전체 높이 */
}
```

**역할**: `#container`를 새창 내부에서 정중앙에 배치

### Container
```css
#container {
  width: 250px;           /* 고정 너비 */
  height: 375px;          /* 고정 높이 */
  padding: 100px 50px;    /* 상하 100px, 좌우 50px */
  border: 1px solid black;
  background: url('../images/notice.jpg') no-repeat left top;
  background-size: auto;
}
```

**실제 콘텐츠 영역**: 150px × 175px (패딩 제외)

---

## 🎨 배경 이미지 설정

```css
background: url('../images/notice.jpg') no-repeat left top;
background-size: auto;
```

- **경로**: CSS 파일 기준 `../images/notice.jpg`
- **위치**: 왼쪽 상단
- **반복**: 반복하지 않음
- **크기**: 원본 크기 유지

---

## 💡 핵심 포인트

### Flexbox 중앙 정렬 작동 원리
1. `display: flex` → body를 플렉스 컨테이너로 설정
2. `justify-content: center` → 주축(수평) 중앙 정렬
3. `align-items: center` → 교차축(수직) 중앙 정렬
4. `min-height: 100vh` → 뷰포트 전체 높이 사용

### 뷰포트 vs 새창 크기
- **새창 크기**: 브라우저 프레임 포함한 전체 창 크기
- **뷰포트**: 실제 웹페이지가 표시되는 영역
- **100vh**: 뷰포트 높이의 100% (새창 크기보다 작음)

---

## 🛠️ 새창 위치 제어 방법

현재 코드에서는 새창 위치가 지정되지 않음. 위치를 제어하려면:

```javascript
// 특정 위치에 창 열기
window.open('notice.html','_blank','width=300, height=400, left=100, top=100')

// 화면 중앙에 창 열기
window.open('notice.html','_blank',
  'width=300, height=400, left=' + (screen.width-300)/2 + 
  ', top=' + (screen.height-400)/2)
```

---

## 📊 최종 결과

새창 내부에서 `#container`가 완벽한 중앙에 배치되어, 깔끔하고 전문적인 공지사항 팝업 창을 구현합니다.