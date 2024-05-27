## promisify

Node.js 中的 `util.promisify` 是一个内置模块 `util` 提供的函数，用于将基于回调的异步函数（即传统的 Node.js 风格函数，接受一个回调函数作为最后一个参数）转换为返回 Promise 的函数。这使得可以更方便地使用异步函数和 Promise 相关的功能，如 async/await。

使用 `util.promisify` 的一般步骤是：

1. 引入 `util` 模块：`const util = require('util');`
2. 使用 `util.promisify` 对目标函数进行转换，这个函数接受一个回调函数作为最后一个参数：`const promisifiedFunction = util.promisify(originalFunction);`
3. 现在 `promisifiedFunction` 是一个返回 Promise 的函数，可以使用 Promise 的方式来调用它：`promisifiedFunction(params).then(...).catch(...);`

### 例如

```javascript
const fs = require('fs');

fs.readFile('example.txt', (err, data) => {
  if (err) {
    console.error('Error reading file:', err);
    return;
  }
  console.log('File contents:', data);
});

```

转换后

```js
const util = require('util');
const fs = require('fs');

const readFileAsync = util.promisify(fs.readFile);

readFileAsync('example.txt')
  .then(data => {
    console.log('File contents:', data);
  })
  .catch(err => {
    console.error('Error reading file:', err);
  });

```

