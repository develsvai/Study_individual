# 프론트 엔드 프로젝트 구조 

```markdown
src
├── @types // TypeScript 사용 시 사용될 데이터들의 타입을 정의 
│   └── @trandmirco
│       └── react-sidenav
│           └── index.d.ts
├── Mock // 테스트 시에 사용될 테스트 데이터들이 존재 
│   ├── CagongIntro
│   │   ├── CagongIntro.module.css
│   │   └── CagongIntro.tsx
│   ├── ImageCardData.tsx
│   └── Reviews.tsx
├── api // 백엔드로 요청할 API들을 정의
│   ├── cagong.api
│   │   ├── cagong.api.tsx
│   │   └── index.tsx
│   ├── index.ts
│   └── user.api
│       ├── index.tsx
│       └── user.api.tsx
├── app // URL로 접근 시 실제 라우팅 될 페이지들이 존재, 단 이 페이지에서는 UI 컴포넌트는 존재하지 않음. SSR, ISR 관련 코드들이 존재, 이 페이지에서 UI 컴포넌트를 호출 및 래핑함.
│   ├── cafedetail
│   │   └── page.tsx
│   ├── cafes
│   │   └── page.tsx
│   ├── global.css // 전역 CSS를 설정하는 파일
│   ├── layout.tsx
│   ├── login
│   │   └── page.tsx
│   ├── mypage
│   │   └── page.tsx
│   ├── page.tsx
│   └── signup
│       └── page.tsx
├── components // 빌드 시 페이지로 생성되지 않는 모든 컴포넌트들이 존재하는 경로
│   ├── common // 재사용 가능한 컴포넌트들이 존재하는 경로
│   │   ├── Button
│   │   │   ├── Button.module.css
│   │   │   └── button.tsx
│   │   ├── Footer
│   │   │   ├── Footer.module.css
│   │   │   └── Footer.tsx
│   │   ├── ImageCard
│   │   │   ├── ImageCard.module.css
│   │   │   └── ImageCard.tsx
│   │   ├── Inputbox
│   │       ├── InputBox.tsx
│   │       └── InputBox.module.css
│   └── pages // /app 경로에서 호출될 페이지들의 UI 컴포넌트 경로
│       ├── CafesPage
│       │   ├── CafesPage.module.css
│       │   └── CafesPage.tsx
│       ├── HomePage
│       │   ├── HomePage.module.css
│       │   └── HomePage.tsx
│       ├── LoginPage
│       │   ├── LoginPage.module.css
│       │   └── LoginPage.tsx
│       └── SignupPage
│           ├── SignupPage.module.css
│           └── SignupPage.tsx
└── styles // 재사용이 가능한 스타일 파일이 존재하는 경로
```

### 경로별 설명

#### `@types`
- 각 페이지 컴포넌트에서 사용할 데이터의 타입 또는 TypeScript의 모듈을 정의함. 프로젝트 전반에서 일관된 타입을 사용할 수 있도록 함.
- 일반적으로 @types는 npm 패키지 이름의 접두사로 사용됨. 프로젝트 내에서는 types 폴더로 명명하는 것이 더 일반적임.


#### `Mock`
백엔드에서 받아온 데이터의 타입과 테스트 데이터들이 모여 있는 곳임. 실제 API가 준비되지 않았을 때 테스트 데이터를 사용하여 개발할 수 있음.
- `CagongIntro` 폴더 안의 `CagongIntro.tsx` 파일은 테스트용 컴포넌트로, 이 컴포넌트의 스타일을 정의하는 `CagongIntro.module.css` 파일도 포함되어 있음.
- Mock 폴더는 테스트용 데이터를 위한 폴더로 적절하지만, 테스트 데이터를 관리하는 방식에 대해 더 명확히 설명할 필요가 있음. 예를 들어, JSON 파일로 데이터를 관리하거나, API 응답을 모킹하는 방식 등을 포함할 수 있음.


#### `api`
백엔드에 요청할 API들이 정의되어 있는 곳임. API 요청 로직을 분리하여 코드의 재사용성을 높이고, 유지보수를 쉽게 함.
- `cagong.api` 폴더와 `user.api` 폴더는 각각 특정 도메인의 API 요청을 정의함. `index.tsx` 파일은 해당 도메인의 API 요청을 모듈화하여 내보내는 역할을 함.
- API 요청 파일의 확장자는 .ts로 충분함. .tsx는 주로 React 컴포넌트 파일에 사용됨. 따라서 API 요청 파일은 .ts로 수정하는 것이 좋음.


#### `app`
URL로 접근 시 실제 라우팅될 페이지들이 존재함. 이 페이지에서는 UI 컴포넌트는 존재하지 않고, SSR 및 ISR 관련 코드들이 있음. 이 페이지에서 UI 컴포넌트를 호출 및 래핑하여 SSR 및 ISR을 구현함.
- `layout.tsx` 파일은 모든 페이지의 공통 레이아웃을 정의함.
- 각 폴더(`cafedetail`, `cafes`, `login`, `mypage`, `signup`)에는 해당 경로로 접근했을 때 렌더링될 페이지 컴포넌트가 있음.

#### `components`
- 빌드 시 페이지로 생성되지 않는 모든 컴포넌트들이 존재하는 경로임.
- components/common은 재사용 가능한 컴포넌트를 포함하지만, 프로젝트의 규모가 커질 경우 도메인별로 컴포넌트를 분리하는 것도 고려할 수 있음.
- 예를 들어, components/common, components/user, components/admin 등으로 나눌 수 있음.

##### `common`
재사용 가능한 컴포넌트들이 존재하는 경로임. Button, Footer, ImageCard, Inputbox 등 프로젝트 전반에서 여러 번 사용되는 컴포넌트들을 모아둠.
- `Button` 폴더는 버튼 컴포넌트와 해당 컴포넌트의 스타일을 정의하는 파일들을 포함함.
- `Footer` 폴더는 푸터 컴포넌트와 해당 컴포넌트의 스타일을 정의하는 파일들을 포함함.
- `ImageCard` 폴더는 이미지 카드 컴포넌트와 해당 컴포넌트의 스타일을 정의하는 파일들을 포함함.
- `Inputbox` 폴더는 입력 상자 컴포넌트와 해당 컴포넌트의 스타일을 정의하는 파일들을 포함함.

##### `pages`
`/app` 경로에서 호출될 페이지들의 UI 컴포넌트 경로임. 각 페이지에 필요한 컴포넌트를 정의하여 `app` 디렉토리의 페이지 파일에서 호출하여 사용함.
- `CafesPage` 폴더는 카페 페이지 컴포넌트와 해당 컴포넌트의 스타일을 정의하는 파일들을 포함함.
- `HomePage` 폴더는 홈 페이지 컴포넌트와 해당 컴포넌트의 스타일을 정의하는 파일들을 포함함.
- `LoginPage` 폴더는 로그인 페이지 컴포넌트와 해당 컴포넌트의 스타일을 정의하는 파일들을 포함함.
- `SignupPage` 폴더는 회원가입 페이지 컴포넌트와 해당 컴포넌트의 스타일을 정의하는 파일들을 포함함.

#### `styles`
재사용 가능한 스타일 파일이 존재하는 경로임. 프로젝트 전반에서 공통으로 사용되는 스타일을 정의하여 스타일의 일관성을 유지하고, 코드의 중복을 줄임.



