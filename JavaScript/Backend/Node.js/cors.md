cors 정책을 회피하기 위해서 사용하는 npm 라이브러리로 yarn install cors를 사용하여 해결할 수 있다.
따라서 이 라이브러리를 사용하면 cors 오류를 없앨 수 있음

```jsx
const express = require('express')
var cors = require('cors')
const app = express()
const port = 3000

app.use(cors()) // 이런식으로 설정하여 cors()를 전역 

app.listen(port, () => {
	console.log(`Example app listening on por ${port}`)
}
```