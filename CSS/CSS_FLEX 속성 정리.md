### Flexbox와 `flex-wrap`

`flex-wrap` 속성은 Flex 컨테이너의 자식 요소들이 컨테이너의 너비를 넘을 경우 자동으로 다음 줄로 이동,

### 예제 HTML 및 CSS 코드

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flexbox 예제</title>
    <style>
        .container {
            display: flex;
            flex-wrap: wrap; /* 자식 요소들이 넘칠 경우 줄 바꿈 */
            border: 2px solid black;
        }
        .item {
            flex: 1 1 30%; /* 요소의 기본 비율을 30%로 설정 */
            margin: 10px;
            padding: 20px;
            background-color: lightblue;
            text-align: center;
            border: 1px solid blue;
        }
        .item.large {
            flex: 1 1 60%; /* 큰 요소의 비율을 60%로 설정 */
        }
        .item.small {
            flex: 1 1 20%; /* 작은 요소의 비율을 20%로 설정 */
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="item small">작은 박스 20%</div>
        <div class="item large">큰 박스 60%</div>
        <div class="item">보통 박스 30%</div>
        <div class="item">보통 박스 30%</div>
        <div class="item small">작은 박스 20%</div>
        <div class="item large">큰 박스 60%</div>
    </div>
</body>
</html>
```

### 과정 설명

1. **컨테이너 설정 (`.container`)**
   - `display: flex;`: 컨테이너를 Flex 컨테이너로 설정
   - `flex-wrap: wrap;`: 자식 요소들이 컨테이너의 너비를 넘을 경우 자동으로 다음 줄로 줄바꿈

2. **기본 아이템 설정 (`.item`)**
   - `flex: 1 1 30%;`: 기본적으로 요소의 비율을 30%로 설정, 이 설정은 각 아이템이 기본적으로 컨테이너 너비의 30%를 차지.
   - `margin`, `padding`, `background-color`, `text-align`, `border` 등을 설정하여 시각적으로 구분.

3. **큰 아이템 설정 (`.item.large`)**
   - `flex: 1 1 60%;`: 큰 요소의 비율을 60%로 설정, 이 설정은 큰 아이템이 컨테이너 너비의 60%를 차지하게 함.

4. **작은 아이템 설정 (`.item.small`)**
   - `flex: 1 1 20%;`: 작은 요소의 비율을 20%로 설정, 이 설정은 작은 아이템이 컨테이너 너비의 20%를 차지하게 함.

### 비율에 따른 배치

- Flexbox의 `flex` 속성은 `flex-grow`, `flex-shrink`, `flex-basis`로 구성됨.
  - `flex-grow`: 요소가 남은 공간을 어떻게 분배받는지 결정
  - `flex-shrink`: 요소가 줄어들 때의 비율을 결정
  - `flex-basis`: 요소의 기본 크기를 설정

- `flex: 1 1 30%`는 `flex-grow: 1`, `flex-shrink: 1`, `flex-basis: 30%`를 의미.

## width 를 이용하는 방법

### 예제 HTML 및 CSS 코드 (width 사용)

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flexbox 예제</title>
    <style>
        .container {
            display: flex;
            flex-wrap: wrap; /* 자식 요소들이 넘칠 경우 줄 바꿈 */
            border: 2px solid black;
        }
        .item {
            width: 30%; /* 요소의 기본 너비를 30%로 설정 */
            margin: 10px;
            padding: 20px;
            background-color: lightblue;
            text-align: center;
            border: 1px solid blue;
            box-sizing: border-box; /* padding과 border를 width에 포함 */
        }
        .item.large {
            width: 60%; /* 큰 요소의 너비를 60%로 설정 */
        }
        .item.small {
            width: 20%; /* 작은 요소의 너비를 20%로 설정 */
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="item small">작은 박스 20%</div>
        <div class="item large">큰 박스 60%</div>
        <div class="item">보통 박스 30%</div>
        <div class="item">보통 박스 30%</div>
        <div class="item small">작은 박스 20%</div>
        <div class="item large">큰 박스 60%</div>
    </div>
</body>
</html>
```

### 과정 설명

1. **컨테이너 설정 (`.container`)**
   - `display: flex;`: 컨테이너를 Flex 컨테이너로 설정
   - `flex-wrap: wrap;`: 자식 요소들이 컨테이너의 너비를 넘을 경우 자동으로 다음 줄로 이동.

2. **기본 아이템 설정 (`.item`)**
   - `width: 30%;`: 기본적으로 요소의 너비를 30%로 설정, 이 설정은 각 아이템이 기본적으로 컨테이너 너비의 30%를 차지하게 함.
   - `box-sizing: border-box;`: padding과 border를 width에 포함.

3. **큰 아이템 설정 (`.item.large`)**
   - `width: 60%;`: 큰 요소의 너비를 60%로 설정, 이 설정은 큰 아이템이 컨테이너 너비의 60%를 차지하게 함.

4. **작은 아이템 설정 (`.item.small`)**
   - `width: 20%;`: 작은 요소의 너비를 20%로 설정, 이 설정은 작은 아이템이 컨테이너 너비의 20%를 차지하게 함.

### 비율에 따른 배치

- 자식 요소의 `width` 속성을 통해 비율을 지정하면 각 요소는 지정된 너비만큼의 공간을 차지함.
- `flex-wrap: wrap` 속성으로 인해 컨테이너의 너비를 넘을 경우 요소들이 자동으로 다음 줄로 이동함.
