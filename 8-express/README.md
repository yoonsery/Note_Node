## Express

**WHY**  
사용자가 많아 검증된 프레임워크, 가볍고 심플하고 유연함

### Express의 특징

#### 사용법

```js
// Express.js

const express = require('express')
const app = express()

app.get('/posts', function (req, res, next) {
  res.send(...)
})

app.post('/posts', function (req, res, next) {
  res.send(...)
})

app.put('/posts/:id', function (req, res, next) {
  res.send(...)
})

app.delete('/posts/:id', function (req, res, next) {
  res.send(...)
})

app.listen(8080)
```

```js
app.get('/posts', function (req, res next) {
  // get 자리에  post, put, delete, all, use 상황에 맞는 함수를 사용하면 된다
  // 첫번째 인자자리에는 URL/Path 정의, 어떤 인자를 처리할 건지 정의
  // 두번째 인자에 callback함수 등록, req, res, next 3개의 인자를 받음
  // 이 콜백함수를 middleware라고 한다
  res.send(...)
})
```

▶️ Express는 middleware의(콜백함수) 연속이다

① `app.use` : `use`는 get, post등 모든 리퀘스트를 처리하는 함수 `* json`로 모든 path를 처리하는 미들웨어 하나 만들고  
② `app.use` : `* headers`, 모든 경로에 대해서 header에 대해서 처리하는 미들웨어 만들고  
③ `app.get` : `/`, root경로에 해당하는 것을 처리하는 미들웨어 만들고  
④ `app.get` : `/posts`라는 미들웨어 만듦  
⑤ `app.use` : `(error)` 라는 미들웨어를 만들어서 각각의 미들웨어들을 체이닝해서 연결해주는 것이 express

- 사용자가 GET 메소드를 이용해서 root 경로에 request가 접수가 되면
  - ①에서는 json을 parsing한 다음 next()를 호출해서 다음 미들웨어로 넘어감
  - ② header에 대한 적절한 처리를 하고 next()로 다음 미들웨어 호출
  - ③ 에서는 리퀘스트가 들어온 GET 메소드와 경로가 동일하기 때문에 여기서 해당하는 리소스를 준비해서 사용자에게 response해준다 `res.send()`
  - ✨ 이렇게 response에 한번 반응을 하면, 그 뒤에있는 미들웨어에게는 넘어가지 않는다
  - 앞에 있던 모든 미들웨어에 해당이 안되면 제일 마지막에 등록된 미들웨어가 호출되고 마지막 미들웨어에서 에러를 던지거나 response를 응답한다