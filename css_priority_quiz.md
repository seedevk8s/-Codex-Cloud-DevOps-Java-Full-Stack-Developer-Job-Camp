# CSS 스타일 우선순위 연습문제

## 문제 1: 기본 우선순위 판단

다음 HTML/CSS 코드에서 "안녕하세요"라는 텍스트의 최종 색상을 예측하시오.

```html
<!DOCTYPE html>
<html>
<head>
    <style>
        p { color: blue; }
        .greeting { color: green; }
        #welcome { color: red; }
    </style>
</head>
<body>
    <p id="welcome" class="greeting">안녕하세요</p>
</body>
</html>
```

**선택지:**
1. blue
2. green  
3. red
4. black (기본값)

---

## 문제 2: !important와 인라인 스타일

다음 코드에서 "프론트엔드 개발자"라는 텍스트의 최종 색상과 글꼴 크기를 예측하시오.

```html
<!DOCTYPE html>
<html>
<head>
    <style>
        .title { 
            color: purple !important; 
            font-size: 16px;
        }
        #main-title { 
            color: orange; 
            font-size: 24px !important;
        }
        h2 { 
            color: navy; 
            font-size: 18px;
        }
    </style>
</head>
<body>
    <h2 id="main-title" class="title" style="color: pink; font-size: 20px;">프론트엔드 개발자</h2>
</body>
</html>
```

**질문:**
- 텍스트 색상: ________
- 글꼴 크기: ________

---

## 문제 3: 복합적 우선순위 시나리오

다음 코드에서 각 단락의 최종 텍스트 색상을 예측하시오.

```html
<!DOCTYPE html>
<html>
<head>
    <style>
        p { color: black; }
        .red-text { color: red; }
        .blue-text { color: blue !important; }
        #special { color: yellow; }
        .red-text.blue-text { color: green; }
    </style>
</head>
<body>
    <p>첫 번째 단락</p>
    <p class="red-text">두 번째 단락</p>
    <p class="blue-text" style="color: orange;">세 번째 단락</p>
    <p id="special" class="red-text">네 번째 단락</p>
    <p class="red-text blue-text">다섯 번째 단락</p>
</body>
</html>
```

**답안 작성:**
1. 첫 번째 단락: ________
2. 두 번째 단락: ________  
3. 세 번째 단락: ________
4. 네 번째 단락: ________
5. 다섯 번째 단락: ________

---

# 모범 답안

## 문제 1 답안
**정답: 3. red**

**해설:**
CSS 우선순위는 다음과 같습니다:
- 태그 선택자 (p): 특이성 점수 1
- 클래스 선택자 (.greeting): 특이성 점수 10  
- ID 선택자 (#welcome): 특이성 점수 100

ID 선택자가 가장 높은 우선순위를 가지므로 텍스트는 빨간색(red)으로 표시됩니다.

---

## 문제 2 답안
**텍스트 색상: purple**
**글꼴 크기: 24px**

**해설:**
- **색상**: `!important`가 붙은 `.title { color: purple !important; }`가 인라인 스타일보다 우선합니다.
- **크기**: `!important`가 붙은 `#main-title { font-size: 24px !important; }`가 인라인 스타일보다 우선합니다.

`!important` > 인라인 스타일 > ID > 클래스 > 태그 순서입니다.

---

## 문제 3 답안
1. 첫 번째 단락: **black**
2. 두 번째 단락: **red**
3. 세 번째 단락: **blue**
4. 네 번째 단락: **yellow**
5. 다섯 번째 단락: **blue**

**해설:**
1. 기본 p 태그 스타일 적용
2. .red-text 클래스 적용
3. .blue-text에 !important가 있어 인라인 스타일보다 우선
4. ID 선택자가 클래스보다 우선순위가 높음
5. .blue-text의 !important가 .red-text.blue-text 조합 선택자보다 우선

---

## 📚 CSS 우선순위 정리

**우선순위 순서 (높음 → 낮음):**
1. `!important` 선언
2. 인라인 스타일 (style 속성)
3. ID 선택자 (#id)
4. 클래스 선택자 (.class), 속성 선택자, 가상 클래스
5. 태그 선택자, 가상 요소
6. 전체 선택자 (*)

**특이성 점수 계산:**
- 인라인 스타일: 1000점
- ID: 100점
- 클래스/속성/가상클래스: 10점  
- 태그/가상요소: 1점

같은 특이성일 경우 나중에 선언된 스타일이 적용됩니다.