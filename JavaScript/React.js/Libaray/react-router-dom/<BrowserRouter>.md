- `<BrowserRouter>` 태그를 사용하여 하위 컴포넌트를 모두 감싸는 방식으로 등록을 하였다.

```jsx
import { BrowserRouter } from 'react-router-dom'

const root = ReactDOM.createRoot(document.getElementById('root'))
root.render(
  <BrowserRouter> // 최상단 root에서 BrowserRouter로 감싸기
      <App /> 
  </BrowserRouter>
)
```

- 이후 [[<Routes>]]와 [[<Route>]]를 사용하여 모든 패스를 묶어주었다.

```jsx
import { Route, Routes } from 'react-router-dom'

const App = () => {
  return (
    <Routes>
      <Route path='/' element={<Layout />} > // outlet을 이용한 중첩라우팅
        <Route index element={<Main />} /> 
        <Route path='/pageA' element={<PageA />} />
        <Route path='/pageB' element={<PageB />} />
        <Route path='/pageC' element={<PageC />} />
      </Route>
      </Routes>
  )
}
```
