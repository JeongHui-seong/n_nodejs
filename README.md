# n_nodejs
## Node.js 기본 개념 정리
1. Express
  - Node.js에서 가장 널리 사용되는 웹 서버 프레임워크
  - HTTP 요청과 응답을 간단하게 처리할 수 있도록 라우팅, 미들웨어, 요청/응답 객체 등을 제공
  ```js
    const express = require('express');
    const app = express();

    app.get('/', (req, res) => {
      res.send('Hello, Express!');
    });

    app.listen(3000, () => {
      console.log('Server is running on http://localhost:3000');
    });
  ```

2. Pug
  - Node.js에서 HTML을 더 간결하게 작성할 수 있도록 도와주는 템플릿 엔진
  - 들여쓰기 기반 문법을 사용하며, 변수/반복문/조건문을 HTML 안에 쉽게 삽입 가능
  - React(Next.js), Vue(Nuxt.js), Svelte(SvelteKit) 등 프론트엔드 프레임워크(SSR)로 대체 가능
  ```js
    // views/index.pug
    doctype html
    html
      head
        title= title
      body
        h1 Hello #{name}!

    // server.js
    const express = require('express');
    const app = express();

    app.set('view engine', 'pug');

    app.get('/', (req, res) => {
      res.render('index', { title: 'My Page', name: 'Node.js' });
    });

    app.listen(3000);
  ```

3. app.get()
  - Express에서 HTTP GET 요청을 처리하는 메서드
  - 특정 URL 경로에 요청이 오면 지정한 콜백 함수 실행
  ```js
    app.get('/about', (req, res) => {
      res.send('About Page');
    });
  ```

4. (req, res) => { ... }
  - Express 라우터에서 사용하는 콜백 함수 형식
  - req는 클라이언트 요청 정보, res는 서버 응답 객체 의미
  ```js
    app.get('/user/:id', (req, res) => {
      const userId = req.params.id;
      res.json({ id: userId, name: 'John Doe' });
    });
  ```