# React.lazy,React.Suspense

## React.lazy

- React에서 컴포넌트를 동적으로 임포트(Dynamic Import) 할 수 있게 해주는 함수로 코드 스플리팅(Code Splitting)을 구현하는 핵심기능.

### 주요 특징

- 지연 로딩 : 컴포넌트가 실제로 필요할때까지 로드를 지연시킴
- 번들 크기 최적화 : 컴포넌트를 실제로 필요할때 로드하기 때문에 초기 번들 크기를 줄여 앱의 로딩속도를 향상

## React.Suspense

- React에서 컴포넌트가 렌더링되기전에 기다려야 하는 작업(비동기 작업)이 있을떄 로딩 상태를 선언적으로 처리할 수 있게 해주는 컴포넌트

### 주요 특징

- 선언적 로딩 처리 : 비동기 작업의 로딩 상태를 컴포넌트 레벨에서 처리
- FallBack UI 제공 : 로딩 중에 보여줄 대체 UI를 간단하게 정의
- Error Boundary : 로딩 상태를 상위 컴포넌트에서 캐치해 처리함

## React.lazy와 Suspense의 사용

```js
import React, { Suspense } from 'react';

const LazyComponent = React.lazy(() => import('./LazyComponent'));

function App() {
  return (
    <Suspense fallback={<div>로딩 중...</div>}>
      <LazyComponent />
    </Suspense>
  );
```

> 이때 LazyComponent는 비동기로 로딩되며 로딩되는 동안 fallback props가 화면에 보이게 됨

## 중첩으로도 사용가능

```js
{
  /* 페이지 레벨 Suspense */
}
<Suspense fallback={<PageLoader />}>
  <Routes>
    <Route path="/" element={<HomePage />} />
    <Route
      path="/profile"
      element={
        // 컴포넌트 레벨 Suspense
        <Suspense fallback={<ProfileSkeleton />}>
          <ProfilePage />
        </Suspense>
      }
    />
  </Routes>
</Suspense>;
```

## 여러 컴포넌트를 한 번에 감싸는 것도 가능

```js
function App() {
  return (
    <Suspense fallback={<div>모든 컴포넌트 로딩 중...</div>}>
      <LazyHeader />
      <LazyContent />
      <LazyFooter />
    </Suspense>
  );
}
```

## 데이터 페칭과 함께 사용가능(React 18 이상부터)

```js
// 데이터 페칭 라이브러리 (SWR, React Query 등)와 함께
function UserProfile({ userId }) {
  const user = useSWR(`/api/users/${userId}`, { suspense: true }); // suspense를 true로 해줘야 감지가능

  return <div>안녕하세요, {user.name}님!</div>;
}

function App() {
  return (
    <Suspense fallback={<div>사용자 정보 로딩 중...</div>}>
      <UserProfile userId={123} />
    </Suspense>
  );
}
```
