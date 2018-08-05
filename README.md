# Serlina

[![npm](https://img.shields.io/npm/v/serlina.svg)](https://github.com/djyde/serlina)

A progressive React SSR framework that open for server integration.

## Motivation

I love using [Next.js](https://github.com/zeit/next.js/), but most of my projects need to use our own web server framework while Next.js run it own server. So I begin making a SSR framework (core) that like Next.js but open for server implementation. It does all the building, compiling, rendering-to-string things and give the rest render-to-html things to your own web server.

> Of course I know Next.js can [custom server and routing](https://github.com/zeit/next.js#custom-server-and-routing), but while Next.js handle the whole http `context`, [I cannot use it in a high layer web framework](https://github.com/eggjs/egg/issues/328).

## Integrations

- Egg: [egg-serlina](https://github.com/serlina-community/egg-serlina)

## Quick Start

```
npm i serlina react react-dom --save
```

Create a folder structure like:

```bash
├── index.js
├── page
│   └── page1.js
```

```js
// page/page1.js

export default () => {
  return <div>Hello Serlina!</div>
}
```

And implement a most simple http server:

```js
// index.js

const { Serlina } = require('serlina')
const path = require('path')

const http = require('http')

const serlina = new Serlina({
  baseDir: path.resolve(__dirname, './')
})

serlina.prepare()
  .then(() => {
    http.createServer(async (req, res) => {
        res.writeHead(200, { 'Content-Type': 'text/html' })
        if (req.url === '/page1') {
          const rendered = await serlina.render('page1')
          res.write(rendered.string)
        } else {
          res.write('works!')
        }
        res.end()
    }).listen(8090)
  })
  .catch(console.error)
```

Open `http://localhost:8090/page1`, you will see the page you wrote in React!

## Documentation

Visit [Doc](https://github.com/djyde/serlina/blob/master/docs/README.md)

## Development

```
npm i

cd test/sample && npm i

npm start # start compiling in watch mode

npm build # compile 

npm test # run a sample web server

npm run doc # run the doc
```

# License

MIT License
