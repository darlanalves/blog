# Making an HTTP request using Node.JS API

For cases when you just need to make an HTTP request and you don't wanna install dependencies, here's a snippet:

```js
// if you're using ESModule mode, use this instead:
// import * as http from 'http';

// CommonJS mode
const http = require('http');

function request(url) {
  return new Promise((resolve, reject) => {
    const request = http.get(new URL(url));
    let buffer = [];
    request.on('response', (res) => {
      res.on('data', (data) => buffer.push(data));
      res.on('error', reject);
      res.on('end', () => resolve(Buffer.concat(buffer).toString()));
    });

    request.on('error', reject);
    request.end();
  });
}
```

This is a very, very simple snippet and won't work well in all scenarios.

[Read more about HTTP requests in Node.JS](https://nodejs.org/en/docs/guides/anatomy-of-an-http-transaction/) if you want to understand/enhance the snippet above.
