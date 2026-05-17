- 커스텀 훅(Custom Hook)은 [[Hooks]]의 로직을 별도 [[함수(Function)]]로 추출하여 컴포넌트 간에 재사용할 수 있게 만든 것이다.
- 이름은 반드시 `use`로 시작해야 하며, 내부에서 다른 훅(`useState`, `useEffect` 등)을 호출할 수 있다.
- 상태 관리 로직, API 호출 로직, 이벤트 리스너 등 반복되는 로직을 컴포넌트 밖으로 분리할 때 사용한다.
- 커스텀 훅은 클래스형 컴포넌트의 [[생명 주기(Life Cycle)]] 메서드나 [[HOC(Higher-Order-Components)]]를 대체하는 함수형 방식이다.
- 보통 `src/hooks/` 디렉터리를 만들고 `use`로 시작하는 파일명으로 작성한다.

## 훅의 규칙 (Rules of Hooks)

- 반드시 **최상위 레벨에서만** 훅을 호출해야 한다. 조건문, 반복문, 중첩 함수 안에서 호출 금지.
- 반드시 **React 함수 컴포넌트 또는 커스텀 훅 내부에서만** 호출해야 한다.
- 이 규칙을 어기면 React가 훅 호출 순서를 추적하지 못해 버그가 발생한다.

## 기본 구조

커스텀 훅은 [[함수(Function)]] 안에서 [[useState()]], [[useEffect()]], [[useReducer()]], [[useCallback()]] 등 [[Hooks]]를 사용하여 원하는 기능을 구현하고, [[컴포넌트(Component)]]에서 사용할 [[값(value)]]들을 반환한다.

```js
function use훅이름(파라미터) {
  // 내부에서 다른 훅 사용
  const [state, setState] = useState(초기값);

  useEffect(() => {
    // 로직
  }, [의존성]);

  // 컴포넌트에서 필요한 값/함수 반환
  return { state, setState };
}
```

## 기본 예시: useFetch

API 호출 로직을 커스텀 훅으로 분리하는 예시:

```js
import { useState, useEffect } from 'react';

function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    let ignore = false;

    async function fetchData() {
      try {
        const res = await fetch(url);
        const json = await res.json();
        if (!ignore) setData(json);
      } catch (err) {
        if (!ignore) setError(err);
      } finally {
        if (!ignore) setLoading(false);
      }
    }

    fetchData();
    return () => { ignore = true; }; // cleanup: 언마운트 시 중복 업데이트 방지
  }, [url]);

  return { data, loading, error };
}

// 사용
function UserList() {
  const { data, loading, error } = useFetch('/api/users');
  if (loading) return <p>로딩 중...</p>;
  if (error) return <p>에러: {error.message}</p>;
  return <ul>{data.map(u => <li key={u.id}>{u.name}</li>)}</ul>;
}
```

## useLocalStorage

[[localStorage]]와 [[useState()]]를 연동하는 커스텀 훅:

```js
import { useState } from 'react';

function useLocalStorage(key, initialValue) {
  const [storedValue, setStoredValue] = useState(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch {
      return initialValue;
    }
  });

  const setValue = (value) => {
    try {
      setStoredValue(value);
      window.localStorage.setItem(key, JSON.stringify(value));
    } catch (error) {
      console.error(error);
    }
  };

  return [storedValue, setValue];
}

// 사용
function Settings() {
  const [theme, setTheme] = useLocalStorage('theme', 'light');
  return (
    <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
      현재 테마: {theme}
    </button>
  );
}
```

## useDebounce

입력 지연 처리를 위한 커스텀 훅:

```js
import { useState, useEffect } from 'react';

function useDebounce(value, delay = 300) {
  const [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    const timer = setTimeout(() => setDebouncedValue(value), delay);
    return () => clearTimeout(timer);
  }, [value, delay]);

  return debouncedValue;
}

// 사용 — 검색창에서 API 호출 횟수 줄이기
function SearchInput() {
  const [query, setQuery] = useState('');
  const debouncedQuery = useDebounce(query, 500);

  useEffect(() => {
    if (debouncedQuery) {
      // 500ms 후에만 API 호출
      fetchSearchResults(debouncedQuery);
    }
  }, [debouncedQuery]);

  return <input value={query} onChange={e => setQuery(e.target.value)} />;
}
```

## useToggle

불리언 상태를 토글하는 간단한 커스텀 훅:

```js
import { useState, useCallback } from 'react';

function useToggle(initialValue = false) {
  const [value, setValue] = useState(initialValue);

  const toggle = useCallback(() => {
    setValue(prev => !prev);
  }, []);

  return [value, toggle];
}

// 사용
function Modal() {
  const [isOpen, toggleModal] = useToggle(false);
  return (
    <>
      <button onClick={toggleModal}>모달 열기/닫기</button>
      {isOpen && <div className="modal">모달 내용</div>}
    </>
  );
}
```

## useInput

폼 입력 상태를 관리하는 커스텀 훅:

```js
import { useState } from 'react';

function useInput(initialValue = '') {
  const [value, setValue] = useState(initialValue);

  const onChange = (e) => setValue(e.target.value);
  const reset = () => setValue(initialValue);

  return { value, onChange, reset };
}

// 사용
function LoginForm() {
  const email = useInput('');
  const password = useInput('');

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log(email.value, password.value);
    email.reset();
    password.reset();
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="email" {...email} />
      <input type="password" {...password} />
      <button type="submit">로그인</button>
    </form>
  );
}
```

## 커스텀 훅의 장점

- **로직 재사용**: 여러 [[컴포넌트(Component)]]에서 같은 로직을 공유할 수 있다.
- **관심사 분리**: 컴포넌트는 UI에 집중하고, 로직은 커스텀 훅이 담당한다.
- **테스트 용이성**: 훅 단위로 독립 테스트가 가능하다.
- **가독성**: 컴포넌트 내 [[useEffect()]], [[useState()]]가 줄어들어 코드가 간결해진다.
- [[HOC(Higher-Order-Components)]]나 [[Render Props]] 패턴보다 구성이 단순하다.

## HOC와 비교

| 구분 | 커스텀 훅 | [[HOC(Higher-Order-Components)]] |
|------|-----------|----------------------------------|
| 사용 대상 | 함수형 컴포넌트 | 함수형/클래스형 모두 |
| 구성 방식 | 훅 호출 | 컴포넌트 래핑 |
| props 충돌 | 없음 | 발생 가능 |
| 디버깅 | 직관적 | 래퍼 중첩으로 복잡 |

## 관련 개념

- [[Hooks]]
- [[useState()]]
- [[useEffect()]]
- [[useReducer()]]
- [[useCallback()]]
- [[컴포넌트(Component)]]
- [[함수(Function)]]
