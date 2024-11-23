- [[Ajax(Asynchronous JavaScript and XML)]] 요청 시 Response를 자동적으로 [[JSON(Java Script Object Notation)]]으로 바꿔서 보내줌으로 axios 라이브러리를 사용하는게 편하다.
- axios 라이브러리는 [[Promise]] [[객체(Object)]]를 공식적으로 지원하기 때문에 [[async await]] 문법이랑 같이 사용할 수 있다.

- axios 라이브러리를 이용하여 이미지나 영상을 업로드하고 싶은 경우, 즉 [[HTML(Hyper Text Markup Language)]] [[<form>]] 태그에 담긴 데이터를 [[Ajax(Asynchronous JavaScript and XML)]] 요청으로 보내고 싶은 경우에는 [[FormData]] [[객체(Object)]]를 이용한다.

## HTML에서 사용하는 방법

- [[HTML(Hyper Text Markup Language)]]에서도 아래 코드처럼 스크립트를 추가하면 사용할 수 있다.

```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script>
	// 로직 작성
</script>
```


## 문법
### axios(config)

- config 설정을 axios()에 전달하여 요청한다.
- url 속성만 필수이며 나머지 속성은 옵션이다.
- method가 지정되지 않으면 GET으로 기본 설정된다.

### [[매개변수(parameter)]]

- Method
- Url
- Data (optional)
- Params (optional)

## axios의 기본 메서드

###  axios.get(url`[, config]`)  

- 데이터 조회를 할 때 사용한다. (GET)
- 사용상황에 따라 params: {} [[객체(Object)]]가 존재할지 안할지가 결정된다.

- 단순 데이터(페이지 요청, 지정된 요청) 요청을 수행할 경우에는 params 객체가 필요 없다.

```javascript
// callback 을 사용할 때,
axios.get("url")
    .then(response =>  {
         // response  
    }).catch(error => {
        // 오류발생시 실행
    }).then(() => {
        // 항상 실행 혹은 finally() 사용
    });


// async await 함수를 사용할 때, 
try {
	const data = await axios.get("url");
} catch {
	// 오류 발생시 실행
}
```

- [[매개변수(parameter)]] 데이터를 포함시키는 경우이다.
- 예를 들어 사용자 번호에 따른 조회가 있다.

```javascript
axios.get("url", {
        params: {
          	id: 123
        }
    })
    .then(response => {
         // response  
    }).catch(error => {
        // 오류발생시 실행
    }).then(() => {
        // 항상 실행
    });
   
   
// async await 함수를 사용할 때, 
try {
	const data = await axios.get("url", 
         params: { 
			id: 123 
		 }
    );
} catch {
	// 오류 발생시 실행
}
```


### axios.post(url, data`[, config]`)  

- 데이터 등록할 때 사용한다.(POST)
- post 메서드에는 일반적으로 데이터를 `Message Body`에 포함시켜서 보낸다.
- get 메서드에서 params를 사용한 경우와 비슷하게 수행된다.

```javascript
axios.post("url", {
         username: "",
         password: ""
    })
    .then(response => {
         // response  
    }).catch(error => {
        // 오류발생시 실행
    }).then(() => {
        // 항상 실행
    });
    

// async await 함수를 사용할 때, 
try {
	const data = await axios.post("url", {
         username: "",
         password: ""
    });
} catch {
	// 오류 발생시 실행
}
```

### axios.put(url, data`[, config]`)  

- 데이터를 수정할 때 사용한다. (PUT)

- put 메서드는 서버 내부적으로 get에서 post 변환되는 과정을 거치기 때문에 post 메서드와 비슷한 형태이다.

```javascript
axios.put("url", {
         username: "",
         password: ""
    })
    .then(response => {
         // response  
    }).catch(error => {
        // 오류발생시 실행
    }).then(() => {
        // 항상 실행
    });


// async await 함수를 사용할 때, 
try {
	const data = await axios.put("url", { 	
      	 username: "", 
         password: ""
    });
} catch {
	// 오류 발생시 실행
}
```

### axios.delete(url`[, config]`)

- 데이터를 제거할 때 사용한다. (DELETE)
- delete 메서드에는 일반적으로 body가 비어있다.

- 그래서 형태는 get과 비슷한 형태를 띄지만 한 번 delete 메서드가 서버에 들어가게 된다면 서버 내에서 삭제 process를 진행하게 된다.

```javascript
axios.delete('/user?ID=12345')
  .then(response => {
      // handle success
      console.log(response);
  })
  .catch(error => {
      // handle error
      console.log(error);
  })
  .then(() => {
      // always executed
  });
 

// async await 함수를 사용할 때, 
try {
	const data = await axios.delete("url");
} catch {
	// 오류 발생시 실행
}
```

### 많은 데이터를 요청할 경우

- query나 params가 많아져서 헤더에 많은 정보를 담을 수 없을 때는 두 번째 인자에 data를 추가해줄 수 있다.

```javascript
axios.delete('/user?ID=12345',{
      data: {
        post_id: 1,
        comment_id: 5,
        username: "april"
      }
})
  .then(response => {
      // handle success
      console.log(response);
  })
  .catch(error => {
      // handle error
      console.log(error);
  })
  .then(() => {
      // always executed
  });
 

// async await 함수를 사용할 때, 
try {
	const data = await axios.delete("url");
} catch {
	// 오류 발생시 실행
}
```

## [[리액트(React)]]에서 Axios 사용하기

- 보통 프로젝트에서는 아래와 같은 과정으로 진행된다.
	- API의 스펙, 서버의 주소, credentials 설정을 담당하는 API.js 라는 파일 생성한다.
	- axios의 기본 설정에 대해서 지정한다.
	- 각각의 서비스에서 가져가 사용하는 형태로 구현한다.

### axios 의 인스턴스 생성/async await 통신

- 먼저 axios의 [[인스턴스(Instance)]]를 생성해서 API라는 변수에 담고 API를 반환시킨다.

```jsx
// API.js
// axios 의 인스턴스를 생성
import axios from 'axios';

const API = axios.create({
	BASE_URL: '',
    headers: {
      	'Content-Type': 'application/json',
    },
    withCredentials: true,
});

export default API;
```

### 각각 파일에서는 [[async await]]으로 [[콜백 함수(Callback Function)]]을 처리하여 통신

```jsx
import API from '../utils/API';

export const login = async (code) => {
    const { data } = await API.post('url',
	    JSON.stringify(code) // JSON 형식을 문자열로 변환
    );
    return data;
}
```

### Error Handling을 위한 try-catch문 사용

```jsx
import API from '../utils/API';

export const refresh = async () => {
    try {
        const { data } = await API.get('url');
    	return data;
    } catch {
    	// Error Handling
    }
};
```

## [[useState()]]로 요청 관리

- 요청에 대한 상태([[state]])를 관리하기 위해서는 3가지 상태를 관리해줘야한다.
	1. 요청의 결과(data)
	2. 로딩 상태(loading)  
	3. 에러(error)

```jsx
import React, { useState, useEffect } from 'react';
import axios from 'axios';

function Users() {
	const [users, setUsers] = useState(null);
	const [loading, setLoading] = useState(false);
	const [error, setError] = useState(null);
	
	const fetchUsers = async () => {
	    try {
	      // 요청이 시작 할 때에는 error 와 users 를 초기화하고
	      setError(null);
	      setUsers(null);
	      // loading 상태를 true 로 바꿈
	      setLoading(true);
	      
	      const response = await axios.get(
		      'https://jsonplaceholder.typicode.com/users'
	      );
	      
	      setUsers(response.data); // 데이터(state)는 response.data안에 들어있음
	    } catch (e) {
	      setError(e);
	    }
	    setLoading(false); // 로딩이 끝났으니 false로 변경
	};
	
	useEffect(() => { // 컴포넌트가 처음 마운트 된 후 실행
		fetchUsers();
	}, []);
	
	if (loading) return <div>로딩중..</div>; // 만약 loading이 True이면
	if (error) return <div>에러가 발생했습니다</div>; // 만약 error가 True이면
	
	// 아직 users가 받아와 지지 않았을 때는 아무것도 표시되지 않도록 해줌
	if (!users) return null;
	
	// 위의 로직에 아무것도 걸리지 않았다면 users가 성공적으로 받아와 진 상태
	
	return (
		<>
		    <ul>
			    {users.map(user => (
			        <li key={user.id}>
				        {user.username} ({user.name})
			        </li>
				))}
		    </ul>
			
			{/* button을 클릭하면 API를 다시 불러와줌 */}
			<button onClick={fetchUsers}>다시 불러오기<button>
		</>
  );
}

export default Users;
```

## [[useReducer()]]로 요청 관리

- useReducer로 관리할 때의 장점이 있따.
	- 로직의 재사용이 가능하다.
	- 컴포넌트 내부의 코드가 짧아진다.(그러나 reducer 함수의 추가로 전체적인 코드의 길이는 늘어난다.)

- 밑의 코드는 reduce를 선언하는 코드이다.

```javascript
function reducer(state, action) {
  switch (action.type) {
	// 발생할 수 있는 상황 LOADING, SUCCESS, ERROR에 대한 case를 만들어줌
	// 로딩중 상태 업데이트
    case 'LOADING':
      return {
        loading: true,
        data: null,
        error: null
      };
	
	// 불러오는데에 성공했을 때는 action.data를 저장해줌
    case 'SUCCESS':
      return {
        loading: false,
        data: action.data,
        error: null
      };
	
	// 에러가 발생하면 action.error를 전달해줌
    case 'ERROR':
      return {
        loading: false,
        data: null,
        error: action.error
      };
    
    default: // 아무 액션도 주어지지 않으면
      throw new Error(`Unhandled action type: ${action.type}`);
  }
```

- 밑에 코드는 useReducer를 사용하는 예시이다.

```jsx
function Users() {
	// reducer 함수의 전달과 초기상태를 정의합니다.
  const [state, dispatch] = useReducer(reducer, {
    loading: false,
    data: null,
    error: null
  });

  const fetchUsers = async () => {
		//시작할 때 로딩중인 상태를 만들어줍니다.
    dispatch({ type: 'LOADING' });
    try {
      const response = await axios.get(
        'https://jsonplaceholder.typicode.com/users'
      );
      dispatch({ type: 'SUCCESS', data: response.data });
    } catch (e) {
      dispatch({ type: 'ERROR', error: e });
    }
  };

  useEffect(() => {
    fetchUsers();
  }, []);

  // state.data 를 users 키워드로 조회
  const { loading, data: users, error } = state;

  if (loading) return <div>로딩중..</div>;
  if (error) return <div>에러가 발생했습니다</div>;
  if (!users) return null;
  return (
    <>
      <ul>
        {users.map(user => (
          <li key={user.id}>
            {user.username} ({user.name})
          </li>
        ))}
      </ul>
      <button onClick={fetchUsers}>다시 불러오기</button>
    </>
  );
}
```

## axios Interceptors

- Axios 라이브러리 이용해서 서버에 Rest API 요청을 보내거나 응답을 받는 과정에서 가로채어 일을 처리할 수 있게 해준다.

```javascript
import axios from 'axios';

const onFulfilled = (response) => {
    // HTTP status가 2XX일 때 처리하고 싶은 로직이 있다면 여기에서 처리함
		// 데이터 받기에 성공했으므로 받은 response를 그대로 return 해준다.
		// 물론 따로 가공해도 됩니다.
    return response;
};
const onRejected = (error) => {
    // HTTP status가 2XX이 아닐 때 여기를 통과하게 됨
    // return은 항상 Promise.reject(error)로 해야함
    return Promise.reject(error);
};

axios.interceptors.response.use(onFulfilled, onRejected); // 미들웨어 형식으로 선언
```

```js
import axios from 'axios';
const customAxiosInstance = () => {
    const axiosInstance = axios.create();
    const onFulfilled = (response) => response;
    const retry = (errorConfig) => {
        return new Promise((resolve) => {
            setTimeout(() => {
                console.log('retry');
                resolve(axiosInstance.request(errorConfig));
            }, 5000);
        });
    }
    const onRejected = (error) => {
        if (error.config) {
            return retry(error.config);
        }
        
        return Promise.reject(error);
    };
    axiosInstance.interceptors.response.use(
        onFulfilled,
        onRejected,
    );
    return axiosInstance;
};
try {
    const apiRequest = customAxiosInstance();
    const response = await apiRequest.get(API_URL);
} catch {
}
```