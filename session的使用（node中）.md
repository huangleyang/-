# session的使用（node中）

#### cookie和sessionhe local Storage 这些都是存在于域名下面 当发送请求给这个域名的时候 会将这些数据携带过去  实现状态的维持



let session = require('express-session')

app.use(session({

​    secret: '我的session我做主', // 相当于是一个加密密钥，值可以是任意字符串

​    resave: false, // 强制session保存到session store中

​    saveUninitialized: false // 强制没有“初始化”的session保存到storage中

  }))

//login post请求

router.post('/login', ctrl.showPostLogin)

配置之后就可以使用 req.session 来获取 创建的

const  showPostLogin = (req, res) => {

​    const body = req.body

​    const sql = 'SELECT * from heimaboke where username = ? and password = ?'

​    //查询失败

​    conn.query(sql, [body.username, body.password], (err, result) => {

​        if (err) return res.send({ status: 502, msg: '登录失败' })

​        if (result.length != 1) return res.send({ status: 501, msg: '登录失败' })

​        req.session.user = result[0]

​        req.session.islogin = true

​        res.send({ status: 200, msg: '登录成功' })

​    })

}