- [[localStorage]]나 [[sessionStorage]]의 데이터가 갱신될 때, storage 이벤트가 실행된다. 

- 여기서 중요한 점은 storage 이벤트가 이벤트를 발생시킨 스토리지를 제외하고 스토리지에서 접근 가능한 [[window]] 객체 전부에서 일어난다는 사실이다.
## storage의 속성

- storage 이벤트는 다음과 같은 [[속성(Property)]]을 가진다.
### key

- 변경된 데이터의 키이다.(.clear()를 호출했다면 null이다.)
### oldValue

- 이전 값이다. (key가 새롭게 추가되었다면 null이다.)
### newValue

-  새로운 값이다.(키가 삭제되었다면 null이다.)
### url 

-  갱신이 일어난 문서의 url이다.
### storageArea

- 갱신이 일어난 [[localStorage]]나 [[sessionStorage]] 객체이다.


## 예시

- 두 개의 창에 같은 사이트를 띄워놨다고 가정해봅시다. 
- 창은 다르지만 localStorage는 서로 공유된다.

- 실제 본 페이지를 두 개의 브라우저 창에 띄워 봅시다.
- 두 창에서 모두 storage 이벤트를 수신하고 있기 때문에 한 창에서 아래 예시를 실행해 데이터를 갱신하면 다른 창에 해당 사항이 반영되는 것을 확인할 수 있다.


```js
// 문서는 다르지만, 갱신은 같은 스토리지에 반영됨

window.onstorage = event => {
	// window.addEventListener('storage', () => {} 와 같다
	
	if (event.key != 'now')
		return;   
		
	alert(event.key + ':' + event.newValue + " at " + event.url);
};

localStorage.setItem('now', Date.now());
```

- storage 이벤트의 또 다른 중요한 특징은 event.url이 있어 데이터가 갱신된 문서의 URL을 알 수 있다는 점이다.

- 또한 event.storageArea에는 스토리지 [[객체(Object)]]가 포함되어 있는데, storage 이벤트는 [[sessionStorage]]나 [[localStorage]]가 변경될 때 모두 발생하기 때문에 event.storageArea는 스토리지 종류에 상관없이 실제 수정이 일어난 것을 참조한다는 것 역시 중요한 특징이다.

- 변경이 일어났을 때 우리는 event.storageArea에 무언가를 설정해 '응답’이 가능하도록 할 수 있다.

- 이런 특징을 이용하면 오리진이 같은 창끼리 메시지를 교환하게 할 수 있다.

모던 브라우저는 오리진이 같은 창끼리 통신할 수 있도록 해주는 [브로드캐스트 채널 API(broadcast channel API)](https://developer.mozilla.org/en-US/docs/Web/API/Broadcast_Channel_API)를 지원합니다. 그런데 이 API는 기능은 풍부하지만, 아직 많은 곳에서 지원하지 않는다는 단점이 있습니다. 단점을 극복하게 해주는 `localStorage` 기반한 폴리필들이 있는데, 이런 라이브러리들은 브라우저와 관계없이 어디서든 창 간 메시지를 교환할 수 있게 해준다는 장점이 있습니다.


웹 스토리지 객체 `localStorage`와 `sessionStorage`를 사용하면 브라우저에 키-값 쌍을 저장할 수 있습니다. 이때,

- `키`와 `값`은 반드시 문자열이어야 합니다.
- 제한 용량은 5MB 이상인데, 브라우저에 따라 다를 수 있습니다.
- 파기되지 않습니다.
- 오리진(도메인·포트·프로토콜)에 묶여있습니다.

|`localStorage`|`sessionStorage`|
|---|---|
|오리진이 같은 탭, 창 전체에서 공유됩니다.|오리진이 같은 브라우저 탭, iframe에서 공유됩니다.|
|브라우저를 껐다 켜도 남아있습니다.|페이지를 새로 고침 해도 남아있습니다. 하지만 탭이나 브라우저를 종료하면 사라집니다.|

API:

- `setItem(key, value)` – 키-값 쌍을 보관합니다.
- `getItem(key)` – 키에 해당하는 값을 받아옵니다.
- `removeItem(key)` – 키와 해당 값을 삭제합니다.
- `clear()` – 모든 것을 삭제합니다.
- `key(index)` – `인덱스`에 해당하는 키를 받아옵니다.
- `length` – 저장된 항목의 개수를 얻습니다.
- `Object.keys`를 사용해 키 전체를 얻을 수 있습니다.
- 객체 프로퍼티처럼 키에 접근할 수 있는데, 이 경우 `storage` 이벤트가 발생하지 않습니다.

storage 이벤트:

- `setItem`, `removeItem`, `clear`를 호출할 때 발생합니다.
- 연산(`key/oldValue/newValue`)과 관련된 데이터 전체와 문서 `url`, 스토리지 객체 `storageArea`를 가지고 있습니다.
- 이벤트가 생성된 곳을 제외하고 스토리지에 접근하는 모든 `window` 객체에서 일어납니다(`sessionStorage`는 탭 내에서, `localStorage`에서는 전역에서).