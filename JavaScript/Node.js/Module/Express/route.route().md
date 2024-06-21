- 일반적으로 주소는 같지만 다른 [[HTTP(Hyper Tranfer Protocol)]] 메서드가 있을 때는 [[express.Router()]]를 사용하는 방식으로 사용하지만 route [[인스턴스(Instance)]]를 사용해서 route.route() [[메서드(Method)]]를 사용해 같은 주소의 메서드끼리 묶을 수 있다.

## route() 메서드로 그룹화하기


```js
router.route('/abc')
	.get((req, res) => {
		res.send('GET /abc');
	})
	.post((req, res) => {
		res.send('POST /abc');
	});
```

