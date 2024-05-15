#### nextjs 中保存文件表单 post 过来的 File 类型的文件
```js
const file = formData.get("avatar");
const bytes = await file.arrayBuffer();
const buffer = Buffer.from(bytes);
const path = `/tmp/${file.name}`;
await writeFile(path, buffer);
console.log(`open ${path} to see the uploaded file`);
```
