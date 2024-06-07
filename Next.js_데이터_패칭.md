 ```markdown
# Next.js 데이터 패칭 정리

Next.js에서는 다양한 방식으로 데이터를 패칭할 수 있습니다. 주요 데이터 패칭 메소드에는 `getStaticProps`, `getServerSideProps`, `getStaticPaths`, 그리고 클라이언트 사이드 데이터 패칭이 있습니다. 각 방법은 특정 상황에 따라 유용합니다.

## 1. `getStaticProps`
`getStaticProps`는 빌드 타임에 데이터를 패칭하여 정적 페이지를 생성하는 데 사용됩니다. 주로 블로그 포스트나 제품 목록 등 변경되지 않는 콘텐츠에 적합합니다.

### 사용 예시
```jsx
export async function getStaticProps() {
  const res = await fetch('https://api.example.com/data')
  const data = await res.json()

  return {
    props: {
      data,
    },
  }
}

const Page = ({ data }) => {
  return (
    <div>
      {/* 렌더링 로직 */}
    </div>
  )
}

export default Page
```

### 특징
- **빌드 타임에 실행**: 페이지가 빌드될 때 실행되며, 결과가 정적으로 생성됩니다.
- **빠른 로드 속도**: 정적 페이지는 서버 요청 없이 빠르게 로드됩니다.
- **재생성**: `revalidate` 옵션을 사용하여 지정된 시간마다 페이지를 다시 생성할 수 있습니다.

## 2. `getServerSideProps`
`getServerSideProps`는 각 요청 시마다 데이터를 패칭하여 페이지를 렌더링합니다. 주로 사용자별 맞춤 정보나 자주 변경되는 데이터를 처리하는 데 적합합니다.

### 사용 예시
```jsx
export async function getServerSideProps() {
  const res = await fetch('https://api.example.com/data')
  const data = await res.json()

  return {
    props: {
      data,
    },
  }
}

const Page = ({ data }) => {
  return (
    <div>
      {/* 렌더링 로직 */}
    </div>
  )
}

export default Page
```

### 특징
- **요청 시 실행**: 각 요청마다 실행되어 최신 데이터를 제공합니다.
- **SEO 친화적**: 서버에서 렌더링된 페이지는 검색 엔진에 최적화됩니다.
- **느린 응답 시간**: 각 요청마다 실행되므로 정적 페이지에 비해 응답 시간이 느릴 수 있습니다.

## 3. `getStaticPaths`
`getStaticPaths`는 동적 라우트와 함께 사용되며, 어떤 페이지를 사전 렌더링할지 지정합니다. 주로 블로그 포스트나 제품 페이지 등 동적 경로가 필요한 콘텐츠에 적합합니다.

### 사용 예시
```jsx
export async function getStaticPaths() {
  const res = await fetch('https://api.example.com/paths')
  const paths = await res.json()

  return {
    paths: paths.map(path => ({ params: { id: path.id } })),
    fallback: false,
  }
}

export async function getStaticProps({ params }) {
  const res = await fetch(`https://api.example.com/data/${params.id}`)
  const data = await res.json()

  return {
    props: {
      data,
    },
  }
}

const Page = ({ data }) => {
  return (
    <div>
      {/* 렌더링 로직 */}
    </div>
  )
}

export default Page
```

### 특징
- **동적 경로 지원**: 사전 정의된 경로를 기반으로 페이지를 생성합니다.
- **정적 페이지**: 생성된 페이지는 정적으로 제공됩니다.
- **Fallback 옵션**: `fallback`을 사용하여 빌드 시 미리 렌더링되지 않은 경로에 대한 처리를 지정할 수 있습니다.

## 4. 클라이언트 사이드 데이터 패칭
클라이언트 사이드 데이터 패칭은 페이지가 로드된 후 클라이언트에서 데이터를 패칭합니다. 주로 사용자 인터랙션 후 데이터를 업데이트할 때 사용됩니다.

### 사용 예시
```jsx
import { useEffect, useState } from 'react'

const Page = () => {
  const [data, setData] = useState(null)

  useEffect(() => {
    const fetchData = async () => {
      const res = await fetch('https://api.example.com/data')
      const data = await res.json()
      setData(data)
    }

    fetchData()
  }, [])

  return (
    <div>
      {/* 렌더링 로직 */}
    </div>
  )
}

export default Page
```

### 특징
- **클라이언트에서 실행**: 페이지가 로드된 후 클라이언트에서 데이터를 패칭합니다.
- **인터랙티브 페이지**: 사용자 인터랙션 후 데이터를 업데이트할 수 있습니다.
- **초기 로딩 속도**: 초기 로딩 속도가 빠르지만, 데이터를 가져오기 전까지 빈 페이지가 표시될 수 있습니다.

## 참고 문서
- [Next.js 공식 데이터 패칭 문서](https://nextjs.org/docs/basic-features/data-fetching)
- [getStaticProps](https://nextjs.org/docs/basic-features/data-fetching/get-static-props)
- [getServerSideProps](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props)
- [getStaticPaths](https://nextjs.org/docs/basic-features/data-fetching/get-static-paths)
- [클라이언트 사이드 데이터 패칭](https://nextjs.org/docs/basic-features/data-fetching/client-side)
```
```
