- indexOf [[메서드(Method)]]는, 문자열(string)에서 특정 문자열(searchvalue)을 찾고, 검색된 문자열이 '첫번째'로 나타나는 위치 index를 반환한다.


## 문법

- 찾는 문자열이 없으면 -1을 반환한다.
- 문자열을 찾을 때 대소문자를 구분한다.

```js
string.indexOf(searchvalue, position)
```

### searchvalue

- 필수 입력값, 찾을 문자열이다.
### position(optional)

- 기본값은 0, string에서 searchvalue를 찾기 시작할 위치이다.
