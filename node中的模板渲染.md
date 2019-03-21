# node中的模板渲染

## ejs

1. 安装 ejs 模板引擎` npm i ejs -S`
2. 使用 app.set() 配置默认的模板引擎 `app.set('view engine', 'ejs')`
3. 使用 app.set() 配置默认模板页面的存放路径 `app.set('views', './views')`
4. 使用 res.render() 来渲染模板页面`res.render('index.ejs', { 要渲染的数据对象 })`，注意，模板页面的 后缀名，可以省略不写！

```
let express = require('express')
const app = express()
app.set('view engine','ejs')
app.set('views', './art1')
app.get('/', (req,res) => {
    res.render('index1.ejs',{name: 'yuan', hobby: ['上网', '睡觉']})
})
app.listen(3000, () => {
    console.log('成功')
})
其中在ejs中模板的使用是
 <%= name%>
   <% hobby.forEach(item => { %>
    <p><%= item %></p>
  <% }) %>
```

## art-template

1. 安装 两个包 `cnpm i art-template express-art-template -S`

2. 自定义一个模板引擎  `app.engine('自定义模板引擎的名称', 渲染函数)`

3. 将自定义的模板引擎，配置为 express 的默认模板引擎  `app.set('view engine', '具体模板引擎的名称')`

4. 配置 模板页面得存放路径 `app.set('views', '路径')`

   ```
   let express = require('express')
   const app = express()
   app.engine('html', require('express-art-template'))
   app.set('view engine', 'html')
   app.set('views', './art1')
   app.get('/', (req,res) => {
       res.render('index.html',{name: 'yuan', hobby: ['上网', '睡觉']})
   })
   app.listen(3000, () => {
       console.log('成功')
   })
   其中在art-template中模板的使用是
    <p>{{name}}</p>
       {{each hobby}}
       {{$value}}
       {{/each}}
   ```

