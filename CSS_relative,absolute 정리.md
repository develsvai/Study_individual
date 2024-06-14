`position: relative;`와 `position: absolute;`는 CSS에서 요소의 위치를 조정할 때 사용하는 속성

### `position: relative;`

- **특징**: 요소를 일반적인 문서 흐름에 따라 배치한 후, 자기 자신의 원래 위치를 기준으로 이동시킴.
- **영향**: 요소가 이동해도 원래 위치가 차지한 공간은 그대로 유지됨.

### `position: absolute;`

- **특징**: 요소를 일반적인 문서 흐름에서 제거하고, 가장 가까운 조상 요소 중 `position` 속성이 `relative`, `absolute`, `fixed`로 설정된 요소를 기준으로 배치합니다. 만약 그런 조상 요소가 없다면, 초기 컨테이닝 블록(보통 `html` 요소)을 기준으로 배치함.
- **영향**: 요소가 문서 흐름에서 제거되므로 원래 위치가 차지한 공간이 사라짐.

### 예제 HTML 및 CSS 코드

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Position 예제</title>
    <style>
        .relative-container {
            position: relative;
            width: 300px;
            height: 300px;
            background-color: lightblue;
            border: 2px solid blue;
        }
        .absolute-child {
            position: absolute;
            top: 50px;
            left: 50px;
            width: 100px;
            height: 100px;
            background-color: lightcoral;
            border: 2px solid red;
        }
    </style>
</head>
<body>
    <div class="relative-container">
        <div class="absolute-child">
            절대 위치
        </div>
        상대 위치 컨테이너
    </div>
</body>
</html>
```

### 과정 설명

1. **상대 위치 컨테이너 설정 (`.relative-container`)**
   - `position: relative;`: 이 속성을 통해 컨테이너는 자신의 위치를 기준으로 자식 요소의 절대 위치를 설정 가능.
   - `width: 300px; height: 300px;`: 컨테이너의 크기를 설정.

2. **절대 위치 자식 요소 설정 (`.absolute-child`)**
   - `position: absolute;`: 자식 요소는 가장 가까운 `position: relative;`를 가진 조상 요소를 기준으로 배치.
   - `top: 50px; left: 50px;`: 자식 요소는 상대 위치 컨테이너의 왼쪽 상단 모서리로부터 50px 아래, 50px 오른쪽에 배치.
   - `width: 100px; height: 100px;`: 자식 요소의 크기를 설정

### 시각적 설명

- `.relative-container`는 문서 흐름에 따라 배치.
- `.absolute-child`는 100px x 100px 크기의 빨간색 상자로, `.relative-container`의 기준에 따라 `top: 50px; left: 50px;` 위치에 배치.
- `.absolute-child`는 `.relative-container`의 위치를 기준으로 하여 이동되므로, `.relative-container`의 내부에 배치됨.

### 결론

- `position: relative;`는 요소 자체의 위치를 기준으로 다른 요소들을 상대적으로 배치하는 데 사용.
- `position: absolute;`는 가장 가까운 조상 요소를 기준으로 배치되며, 문서 흐름에서 제거됨.
