# cookie (node设置的cookie)

## 为什么会有cookie的存在

HTTP无状态协议，是指协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。

## 存在的意义 

1.定义：什么是cookie呢。cookie就是存储在客户端的一小段文本

2.cookie是一门客户端的技术 ，因为cookie是存储在客户端浏览器中的

3.cookie的作用，就是为了实现 客户端与服务器之间的状态保持

4.cookie及时，不安全 ， 不要使用cookie保存敏感信息

5.cookie默认，在浏览器关闭之后，就立即失效；如果想指定cookie的过期时间需要设置expires属性

const cookie = { }

req.headers.cookie && req.headers.cookie.split(';').forEach(item => {
const parts = item.split('=')

cookie[parts[0]] = parts[1]

})

获取当前时间 

const expiresTime = new Date(Date.now()+10 *1000).toUTCString

因为设置过期时间只能使用UTC格式所以得用以上方法获取时间

设置cookie

res.writeHeader(200, {'Content-Type': 'text/html; charset=utf-8',

'Set-cookie': ['msg=yes; expires = expiresTime ', 'text=ook']}

)

完整的实例

```
const http = require('http')

const server = http.createServer()

// 需求：如果客户端是第一次请求 服务器，就返回文本 "送你一朵小红花"
// 如果不是第一次请求服务器，就返回文本 "不要太贪心"
server.on('request', (req, res) => {
  if (req.url === '/') {
    // 每次客户端请求服务器，我们都可以解析一下请求头中，是否携带了 cookie，如果携带了cookie，则把 cookie 解析出来，解析为一个对象；
    const cookie = {}
    req.headers.cookie &&
      req.headers.cookie.split('; ').forEach(item => {
        const parts = item.split('=')
        cookie[parts[0]] = parts[1]
      })

    if (cookie.isvisit === 'yes') {
      //客户端之前曾经访问过服务器
      res.writeHeader(200, {
        'Content-Type': 'text/html; charset=utf-8'
      })
      res.end('不要太贪心')
    } else {
      // 客户端之前没有访问过服务器
      const expiresTime = new Date(Date.now() + 10 * 1000).toUTCString()
      res.writeHeader(200, {
        'Content-Type': 'text/html; charset=utf-8',
        'Set-Cookie': ['isvisit=yes; expires=' + expiresTime, 'test=ook']
      })
      res.end('送你一朵小红花')
    }
  } else {
    res.end('404')
  }
})

server.listen(4321, () => {
  console.log('server running at http://127.0.0.1:4321')
})
```

