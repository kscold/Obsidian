- Next에서 router [[객체(Object)]]에 접근하기 위해서 useRouter()를 사용한다.

## 문법

```js
import { useRouter } from 'next/router'  

const router = useRouter()
```

## router.push | router.replace

- 클라이언트에서 페이지 전환을 할때 사용한다.

- push로 이동시키면 history stack에 쌓여서 뒤로가기가 가능하고 replace로 이동시키면 history stack에 안쌓여서 뒤로가기 불가능하다.

```js
router.push(url, as, options)
router.replace(url, as, options)
```

 - 외부 url로 이동하는 경우에는 [[window]].location를 쓰는게 더 낫다.

1. url
- 이동할 url > url [[객체(Object)]] 사용 가능하다.

2. as
- 이동 후 브라우저에 표시될 URL

3. option
      - scroll, default: true  탐색 후 페이지 맨 위로 스크롤 제어한다.
      - shallow, default: false getStaticProps, getServerSideProps, getInitialProps를 다시 실행하지 않고 현재 페이지 경로 업데이트한다.
      - locale, 새로운 페이지의 locale이다.

```js
// as 사용 안하고 option (scroll) 사용하는 경우 예시

router.push({
  pathname: router.pathname,
  query: { ...router.query, currency: newCurrency.value },
}, undefined, { scroll: false });
```

## router.prefetch

- 빠른 클라이언트 전환을 위해 페이지를 데이터를 미리 가져온다. 
- next/link 의 경우 자동으로 페이지를 미리 가져오기 때문에 next/link 가 없는 탐색에서 유용하다.

```js
router.prefetch(url, as)
```

**1. url**

- 이동할 url > url 객체 사용 가능

**2. as**

- 이동 후 브라우저에 표시될 URL

## **router.beforePopState**

라우터가 동작하기전에 무언가 작업을 하고 싶을 때 사용

- popstate : 사용자의 세션 기록 탐색으로 인해 현재 활성화된 기록 항목이 바뀔 때 발생  
**⚠️ popstate 관련 정보** → [https://developer.mozilla.org/ko/docs/Web/API/Window/popstate_event](https://developer.mozilla.org/ko/docs/Web/API/Window/popstate_event)

```
import { useEffect } from 'react'
import { useRouter } from 'next/router'

export default function Page() {
  const router = useRouter()

  useEffect(() => {
    router.beforePopState(({ url, as, options }) => {
      // 아래 두 url로만 이동을 허용하고 싶을때
      if (as !== '/' && as !== '/other') {
        // Have SSR render bad routes as a 404.
        window.location.href = as
        return false
      }

      return true
    })
  }, [])

  return <p>Welcome to the page</p>
}
```

## **router.back**

뒤로가기 버튼 클릭과 같음

 window.history.back()  실행

```
import { useRouter } from 'next/router'

export default function Page() {
  const router = useRouter()

  return (
    <button type="button" onClick={() => router.back()}>
      Click here to go back
    </button>
  )
}
```

## **router.reload**

새로고침 버튼 클릭과 같음

 window.history.reload()  실행

```
import { useRouter } from 'next/router'

export default function Page() {
  const router = useRouter()

  return (
    <button type="button" onClick={() => router.reload()}>
      Click here to reload
    </button>
  )
}
```

## **router.events**

next/router로 이벤트를 감지해서 특정 이벤트가 발생하면 함수를 실행

**1. routeChangeStart**

 **routeChangeStart(url, { shallow })**  경로가 변경되기 시작할때 발생

**2. routeChangeComplete**

 **routeChangeComplete(url, { shallow })**  경로가 완전히 변경되면 발생

**3. **routeChangeError****

 **routeChangeError(url, { shallow })**  경로 변경시 오류가 발생하거나 경로 전환 취소시 발생 (err.cancelled - 탐색이 취소되었는지 여부)

**4. beforeHistoryChange**

 **beforeHistoryChange(url, { shallow })**  브라우저의 history를 변경하기 전에 발생

**5. hashChangeStart**

 **hashChangeStart(url, { shallow })**  해시는 변경되지만 페이지는 변경되지 않을때 발생

**6. hashChangeComplete**

 **hashChangeComplete(url, { shallow })**  해시가 변경되었지만 페이지는 변경되지 않을때 발생

```
import { useEffect } from 'react'
import { useRouter } from 'next/router'

export default function MyApp({ Component, pageProps }) {
  const router = useRouter()

  useEffect(() => {
    const handleRouteChange = (url, { shallow }) => {
      console.log(
        `App is changing to ${url} ${
          shallow ? 'with' : 'without'
        } shallow routing`
      )
    }

    router.events.on('라우터이벤트 이름 ex) routeChangeStart' handleRouteChange)

    // If the component is unmounted, unsubscribe
    // from the event with the `off` method:
    return () => {
      router.events.off('라우터이벤트 이름 ex) routeChangeStart' handleRouteChange)
    }
  }, [])

  return <Component {...pageProps} />
}
```

## **withRouter**

라우터에 의해서 호출된 컴포넌트가 아니어도 match, location, history 객체에 접근할 수 있도록 해준다. 

```
import { withRouter } from 'next/router'

function Page({ router }) {
  return <p>{router.pathname}</p>
}

export default withRouter(Page)
```

#### **+ widthRouter > use TypeScript**

```
import React from 'react'
import { withRouter, NextRouter } from 'next/router'

interface WithRouterProps {
  router: NextRouter
}

interface MyComponentProps extends WithRouterProps {}

class MyComponent extends React.Component<MyComponentProps> {
  render() {
    return <p>{this.props.router.pathname}</p>
  }
}

export default withRouter(MyComponent)
```