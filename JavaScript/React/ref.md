


- 리액트 컨포넌트 안에도 id를 사용할 수 있으나, 같은 [[컴포넌트]]를 재사용하는 방법에 있어, [[DOM(Document Object Model)]]에서 id는 unique(유일)해야 하는데 이런 상황에서 중복 id를 가진 DOM이 여러 개 생기니 잘못된 사용이 된다.

## 바닐라 [[HTML]]에서의 id 사용법

```html
<html lang="en">

<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Example</title>
	<style>
		.success {
			background-color: green;
		}
		.failure {
			background-color: red
		}
	</style>
	<script>
		function vaildate() {
			var input = document.getElementById('password')
			input.className = '';
			
			if (input.value === '0000') {
				input.className = 'success'
			} else {
				input.className = 'failure'
			}
		}
	</script>
</head>

<body>
	<input type="password" id="password"></input>
	<button onclick="vaildate()">Vaildate</button>
</body>

</html>
```