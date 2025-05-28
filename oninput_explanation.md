# oninput 이벤트 작동 원리

## 이벤트 발생 과정

### 1. 사용자 상호작용
- 사용자가 range input(슬라이더)을 드래그하거나 클릭
- 슬라이더의 값이 실시간으로 변경됨

### 2. 브라우저의 이벤트 감지
- 브라우저 엔진이 input 요소의 값 변경을 감지
- `input` 이벤트가 자동으로 생성되고 발생

### 3. 이벤트 핸들러 실행
```javascript
oninput="document.getElementById('selected-value').innerText = this.value"
```

이 코드가 실행되는 과정:
1. **`this.value`**: 현재 이벤트가 발생한 요소(range input)의 현재 값을 가져옴
2. **`document.getElementById('selected-value')`**: DOM에서 id가 'selected-value'인 요소를 찾음
3. **`.innerText = this.value`**: 찾은 요소의 텍스트 내용을 슬라이더 값으로 업데이트

## 왜 이 메소드가 자동으로 호출되는가?

### 이벤트 기반 프로그래밍
- HTML의 `oninput` 속성은 **이벤트 리스너**를 등록하는 방법
- 브라우저가 해당 이벤트(값 변경)를 감지하면 자동으로 등록된 코드를 실행

### DOM 이벤트 모델
```
이벤트 발생 → 이벤트 객체 생성 → 등록된 핸들러 실행
```

### oninput vs 다른 이벤트들
- **`oninput`**: 값이 변경될 때마다 실시간으로 발생 (타이핑, 드래그 중)
- **`onchange`**: 요소의 포커스가 벗어날 때 발생
- **`onclick`**: 클릭할 때만 발생

## 코드 분석

```html
<input type="range" min="0" max="100" step="1" value="20" 
       oninput="document.getElementById('selected-value').innerText = this.value">
<span id="selected-value">20</span>
```

### 핵심 요소들
1. **`type="range"`**: 슬라이더 UI 생성
2. **`oninput="..."`**: 값 변경 시 실행할 JavaScript 코드 정의
3. **`this`**: 이벤트가 발생한 현재 요소(input)를 참조
4. **`document.getElementById()`**: DOM API를 사용해 요소 선택
5. **`.innerText`**: 요소의 텍스트 내용을 변경

## 대안적 구현 방법

### 1. 별도 함수로 분리
```javascript
function updateValue(element) {
    document.getElementById('selected-value').innerText = element.value;
}
```
```html
<input type="range" oninput="updateValue(this)">
```

### 2. addEventListener 사용
```javascript
document.querySelector('input[type="range"]').addEventListener('input', function() {
    document.getElementById('selected-value').innerText = this.value;
});
```

이렇게 브라우저의 이벤트 시스템이 사용자의 행동을 감지하고, 등록된 핸들러를 자동으로 실행하여 실시간으로 UI를 업데이트하는 것입니다.