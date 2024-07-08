```
  src
  ├── @types //typeScript 사용시 사용될 데이터들의 타입을 정의 
  │   └── @trandmirco
  │       └── react-sidenav
  │           └── index.d.ts
  ├── Mock // 테스트시에 사용될 테스트 데이터 들이 존재 
  │   ├── CagongIntro
  │   │   ├── CagongIntro.module.css
  │   │   └── CagongIntro.tsx
  │   ├── ImageCardData.tsx
  │   └── Reviews.tsx
  ├── api // 백엔드 로 요청할 api들을 정의
  │   ├── cagong.api
  │   │   ├── cagong.api.tsx
  │   │   └── index.tsx
  │   ├── index.ts
  │   └── user.api
  │       ├── index.tsx
  │       └── user.api.tsx
  ├── app // url 로 접근시 실제 라우팅 될 페이지들이 존재, 단 이 페이지 에서는 ui 컴포넌트는 존재하지 않음 , ssr, isr 관련 코드들이 존재 , 이 페이지에서 ui 컴포넌트를 호출 및 래핑함.
  │   ├── cafedetail
  │   │   └── page.tsx
  │   ├── cafes
  │   │   └── page.tsx
  │   ├── global.css // 전역 css 를 설정하는 파일 
  │   ├── layout.tsx
  │   ├── login
  │   │   └── page.tsx
  │   ├── mypage
  │   │   └── page.tsx
  │   ├── page.tsx
  │   └── signup
  │       └── page.tsx
  	├── components // 빌드 시 페이지로 생성 되지 않는 모든 컴포넌트 들이 존재하는 경로 
  │   ├── common // 재사용 가능 한 컴포넌트 들이 존재하는 경로 
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
  │   │   │   ├── InpuBox.tsx
  │   │   │   └── InputBox.module.css
  │   │   ├── SearchBar
  │   │   │   ├── SearchBar.module.css
  │   │   │   └── SearchBar.tsx
  │   │   ├── SideNavBar
  │   │   │   ├── SideNav.module.css
  │   │   │   └── SideNavBar.tsx
  │   │   ├── SlideImageCard
  │   │   │   ├── SlideImageCard.module.css
  │   │   │   └── SlideImageCard.tsx
  │   │   ├── TopBanner
  │   │   │   ├── TopBanner.module.css
  │   │   │   └── TopBanner.tsx
  │   │   ├── reviewsCard
  │   │   │   ├── reviewsCard.module.css
  │   │   │   └── reviewsCard.tsx
  │   │   └── test
  │   │       └── page.tsx
  │   └── pages // /app 경로 에서 호출 될 페이지들의 ui 컴포넌트 경로 
  │       ├── CafesPage
  │       │   ├── CafesPage.module.css
  │       │   └── CafesPage.tsx
  │       ├── HomePage
  │       │   ├── HomePage.module.css
  │       │   └── HomePage.tsx
  │       ├── LoginPage
  │       │   ├── LoginPage.module.css
  │       │   └── LoginPage.tsx
  │       └── SiginupPage
  │           ├── SiginupPage.module.css
  │           └── SiginupPage.tsx
  └── styles //재사용이 가능한 스타일 파일이 존재하는 경로 
```
