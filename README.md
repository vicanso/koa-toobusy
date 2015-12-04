# connection limit middlware for koa

## Installation

```bash
$ npm install koa-connection-limit
```

## API

```js
const koa = require('koa');
const koaConnectionLimit = require('koa-connection-limit');
const app = koa();

app.use(koaConnectionLimit({
  mid: 5,
  high: 10,
  throwError: false,
  change: function (status) {
    // status: low, mid, high
    console.info(status);
  }
}));
```
### Options

- `mid` mid connection limit count
- `high` high connection limit count
- `throwError` when `true` or `undefined`, the connection count reach high limit count, it will throw error
- `event` when status change, the event function will be triggered.


## Example

```js
'use strict';
const Koa = require('koa');
const app = new Koa();
const koaConnectionLimit = require('koa-connection-limit');

// logger

app.use((ctx, next) => {
  const start = new Date;
  return next().then(() => {
    const ms = new Date - start;
    console.log(`${ctx.method} ${ctx.url} - ${ms}ms`);
  });
});


app.use(koaConnectionLimit({
  high: 2,
  mid: 1,
  throwError: false,
  change: function changeStatus(status) {
    console.dir(status);
  }
}));


app.use((ctx, next) => {
  const delay = new Promise(function(resolve, reject) {
    setTimeout(resolve, 3000);
  });
  return delay.then(next);
});

// response

app.use(ctx => {
  ctx.body = 'Hello World';
});

app.listen(3000);
```


## License

MIT
