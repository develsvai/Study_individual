### 기본적으로 `box-sizing`의 동작 방식

- **`content-box` (기본값)**: 요소의 너비와 높이는 패딩과 보더를 제외한 콘텐츠 영역의 크기만을 의미, 따라서 패딩과 보더를 추가하면 전체 크기가 커짐.
- **`border-box`**: 요소의 너비와 높이에 패딩과 보더가 포함, 따라서 패딩과 보더를 추가해도 전체 크기는 변경되지 않고, 콘텐츠 영역이 줄어듬.

### 예제 HTML 및 CSS 코드

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Box-sizing 예제</title>
    <style>
        .box {
            width: 200px;
            height: 100px;
            margin: 20px;
            padding: 20px;
            border: 10px solid blue;
            background-color: lightblue;
        }
        .content-box {
            box-sizing: content-box; /* 기본값 */
        }
        .border-box {
            box-sizing: border-box;
        }
    </style>
</head>
<body>
    <div class="box content-box">
        content-box
    </div>
    <div class="box border-box">
        border-box
    </div>
</body>
</html>
```

### 결과 설명

1. **`content-box` 예제 (`.content-box`)**
   - 너비와 높이 설정: `width: 200px; height: 100px;`
   - 패딩: `padding: 20px;`
   - 보더: `border: 10px solid blue;`
   - 실제로 요소의 전체 크기는 너비가 `200px + 20px * 2 (패딩) + 10px * 2 (보더)` = `260px`, 높이가 `100px + 20px * 2 (패딩) + 10px * 2 (보더)` = `160px`이 됨.
   - 이 경우, 패딩과 보더는 너비와 높이에 추가적으로 적용됨.

2. **`border-box` 예제 (`.border-box`)**
   - 너비와 높이 설정: `width: 200px; height: 100px;`
   - 패딩: `padding: 20px;`
   - 보더: `border: 10px solid blue;`
   - 실제로 요소의 전체 크기는 설정한 너비와 높이 그대로 `200px`과 `100px`이 유지됨.
   - 이 경우, 패딩과 보더는 너비와 높이에 포함되어 계산되므로 콘텐츠 영역의 크기가 줄어듬, 콘텐츠 영역의 너비는 `200px - 20px * 2 (패딩) - 10px * 2 (보더)` = `140px`, 높이는 `100px - 20px * 2 (패딩) - 10px * 2 (보더)` = `40px`이 됨.

### 시각적 비교

- `content-box`:
  - 전체 크기: 260px x 160px
  - 콘텐츠 크기: 200px x 100px

- `border-box`:
  - 전체 크기: 200px x 100px
  - 콘텐츠 크기: 140px x 40px

