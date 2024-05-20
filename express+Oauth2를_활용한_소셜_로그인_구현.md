물론입니다. 다음은 TypeScript에서 Express와 OAuth를 사용한 소셜 로그인 구현 과정과 오류 해결 방법을 마크다운 형식으로 정리한 내용입니다.

---

# Express와 OAuth로 소셜 로그인 구현하기

## 1. 프로젝트 구조 설정

```
my-app/
  backend/
    src/
      server.ts
      types/
        index.d.ts
    package.json
    tsconfig.json
  frontend/
    // Next.js or any 다른 프론트엔드 프레임워크
  .gitignore
  package.json
```

## 2. 백엔드 설정

### 2.1. 백엔드 프로젝트 초기화

`backend` 폴더에서 Node.js 프로젝트를 초기화하고 필요한 패키지를 설치합니다.

```bash
cd backend
npm init -y
npm install express passport passport-google-oauth20 cookie-session dotenv
npm install typescript ts-node @types/node @types/express @types/passport @types/passport-google-oauth20 @types/cookie-session --save-dev
```

### 2.2. TypeScript 설정

`backend` 폴더에 `tsconfig.json` 파일을 생성하고 TypeScript 설정을 추가합니다.

```json
{
  "compilerOptions": {
    "target": "ES6",
    "module": "commonjs",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true
  },
  "include": ["src"]
}
```

### 2.3. 환경 변수 설정

`backend` 폴더에 `.env` 파일을 생성하고 다음과 같은 환경 변수를 추가합니다.

```
PORT=3001
GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret
COOKIE_KEY=your-cookie-key
```

### 2.4. 서버 코드 작성

`backend/src` 폴더를 생성하고 `server.ts` 파일을 생성합니다. 다음과 같은 코드를 추가합니다.

```typescript
import express from 'express';
import passport from 'passport';
import { Strategy as GoogleStrategy } from 'passport-google-oauth20';
import cookieSession from 'cookie-session';
import dotenv from 'dotenv';

dotenv.config();

const app = express();

app.use(
  cookieSession({
    name: 'session',
    keys: [process.env.COOKIE_KEY!],
    maxAge: 24 * 60 * 60 * 1000, // 24 hours
  })
);

app.use(passport.initialize());
app.use(passport.session());

passport.serializeUser((user, done) => {
  done(null, user);
});

passport.deserializeUser((obj: any, done) => {
  done(null, obj);
});

passport.use(
  new GoogleStrategy(
    {
      clientID: process.env.GOOGLE_CLIENT_ID!,
      clientSecret: process.env.GOOGLE_CLIENT_SECRET!,
      callbackURL: '/auth/google/callback',
    },
    (token, tokenSecret, profile, done) => {
      return done(null, profile);
    }
  )
);

app.get(
  '/auth/google',
  passport.authenticate('google', { scope: ['profile', 'email'] })
);

app.get(
  '/auth/google/callback',
  passport.authenticate('google', { failureRedirect: '/' }),
  (req, res) => {
    res.redirect('/profile');
  }
);

app.get('/profile', (req, res) => {
  if (!req.isAuthenticated()) {
    return res.redirect('/auth/google');
  }
  res.send(`Hello, ${(req.user as Express.User).displayName}`);
});

app.get('/logout', (req, res) => {
  req.logout((err) => {
    if (err) {
      return next(err);
    }
    res.redirect('/');
  });
});

const PORT = process.env.PORT || 3001;
app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
```

### 3. 사용자 타입 정의 및 오류 해결

#### 3.1. 사용자 타입 정의

`backend/src/types` 폴더를 생성하고 `index.d.ts` 파일을 추가합니다.

```typescript
// backend/src/types/index.d.ts
import { Express } from 'express';

declare global {
  namespace Express {
    interface User {
      displayName?: string;
      // 필요에 따라 추가 속성을 정의합니다.
      // email?: string;
      // id?: string;
      // 등등...
    }
  }
}
```

#### 3.2. 서버 코드에서 타입 적용

`server.ts` 파일에서 `req.user` 객체의 타입을 지정합니다.

```typescript
app.get('/profile', (req, res) => {
  if (!req.isAuthenticated()) {
    return res.redirect('/auth/google');
  }
  res.send(`Hello, ${(req.user as Express.User).displayName}`);
});
```

### 4. 프론트엔드 설정

프론트엔드는 Next.js 또는 원하는 프레임워크를 사용할 수 있습니다. 여기서는 Next.js를 사용합니다.

#### 4.1. Next.js 프로젝트 초기화

루트 디렉토리에서 Next.js 프로젝트를 초기화합니다.

```bash
cd ..
npx create-next-app frontend
```

#### 4.2. 로그인/로그아웃 링크 추가

`frontend/pages/index.js` 파일을 다음과 같이 수정합니다.

```jsx
import React from 'react';

const Index = () => {
  return (
    <div>
      <h1>Welcome to My App</h1>
      <a href="http://localhost:3001/auth/google">Login with Google</a>
    </div>
  );
};

export default Index;
```

#### 4.3. 프로필 페이지 설정

`frontend/pages/profile.js` 파일을 추가하고 다음과 같이 수정합니다.

```jsx
import React, { useEffect, useState } from 'react';

const Profile = () => {
  const [user, setUser] = useState(null);

  useEffect(() => {
    fetch('http://localhost:3001/profile', {
      credentials: 'include',
    })
      .then((res) => res.json())
      .then((data) => setUser(data))
      .catch((error) => console.error('Error:', error));
  }, []);

  return (
    <div>
      {user ? (
        <div>
          <h1>Welcome, {user.displayName}</h1>
          <a href="http://localhost:3001/logout">Logout</a>
        </div>
      ) : (
        <div>Loading...</div>
      )}
    </div>
  );
};

export default Profile;
```

### 5. 실행 및 테스트

1. 백엔드 서버를 실행합니다.

```bash
cd backend
npx tsc
node dist/server.js
```

2. 프론트엔드 서버를 실행합니다.

```bash
cd ../frontend
npm run dev
```

이제 브라우저에서 `http://localhost:3000`에 접속하면 Google 소셜 로그인을 사용할 수 있습니다. 로그인 후, 프로필 페이지로 리디렉션되어 로그인한 사용자 정보를 확인할 수 있습니다.

---
