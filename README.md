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

## Server Setup
- npm init -y 명령어로 package.json 생성
- npm i nodemon -D (nodemon은 개발 중 서버 파일이 수정되면 자동으로 재시작해줌)
- npm i @babel/core @babel/cli @babel/node -D
- npm i @babel/preset-env -D (babel은 자바스크립트를 하위 호환 가능하게 변환해주는 도구)
- npm i express
- npm i pug
- babel.config.json 생성 후 아래 작성
```json
  {
    "presets" : ["@babel/preset-env"]
  }
```
- nodemon.json 생성 후 아래 작성
```json
  {
    "exec" : "babel-node src/server.js"
  }
```
- src/server.js 생성 후 아래 작성
```js
  import express from "express";

  const app = express();

  console.log("hello");

  app.listen(3000);
```

  ### 최선 서버 셋업 방식 (Babel 없이 ES 모듈 사용)
  - Node 12+ 부터는 기본적으로 ES Module(import/export) 지원. 따라서 Babel 없이 가능
  ```bash
    npm init -y
    npm i express pug
    npm i -D nodemon
  ```
  - package.json에 추가
  ```json
    "type" : "module",
    "scripts" : {
      "dev" : "nodemon src/server.js"
    }
  ```

## Frontend Setup
- server.js에 pug, static 폴더 경로 설정, 렌더링 설정
```js
  app.set("view engine", "pug");
  app.set("views", __dirname + "/views");
  app.use("/public", express.static(__dirname + "/public"))
  app.get("/", (req, res) => res.render("home"));
```
- src/public/views/home.pug 생성
```pug
  doctype html
  html(lang="en")
    head
      meta(charset="UTF-8")
      meta(name="viewport", content="width=device-width, initial-scale=1.0")
      title Noom
      link(rel="stylesheet", href="https://unpkg.com/mvp.css")
    body 
      header 
        h1 Noom
      main 
        h2 Welcome to Noom
      script(src="/public/js/app.js") 
```
- nodemon.json에서 <"ignore" : ["src/public/*"],> 를 추가해 프론트에서 저장하면 서버 재실행 안되게 함.
- catchall url을 만들어서 home url만 사용하게 함
```js
  app.get("/*", (req, res) => res.redirect("/"));
```