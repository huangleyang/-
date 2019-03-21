# Promise 同步



```
 $.ajax({	
	url: 'data/json.txt',
	data: 'json',
	success(arr) {
	resolve(arr)
},
	error(err) {
	reject(err)
}
})
})
```



```
  function createPromise(url) {
            return new Promise(function (resolve, reject) {

                $.ajax({
                    url,
                    dataType: 'json',
                    success(arr) {
                        resolve(arr)
                    },
                    error(err) {
                        reject(err)
                    }
                })
            })
        }
        Promise.all([
            createPromise('data/arr.txt'),
            createPromise('data/json.txt')
        ]).then(function (arr) {
            alert('成功了')
            let [res1, res2] = arr
            console.log(res1)
            console.log(res2)
        }, function () {
            alert('失败')
        }
        )
```

jq中的Promise方法

```
        Promise.all([
            $.ajax({url: 'data/arr.txt', dataType: 'json'}),
            $.ajax({url: 'data/json.txt', dataType: 'json'})
        ]).then( res => {
            let [arr, json] = res
            alert('成功了')
            console.log(arr,json)
        }, err => {
            alert('失败了',err)
        }
    )
```

解决回调地狱	

```
const fs = require('fs')
const path = require('path')
function creatPromise(url) {
    return new Promise(function (resolve,reject) {
        fs.readFile(path.join(__dirname, url), 'utf8', function (err, data) {
            if(err) return reject(err)
            resolve(data)
        })
    })
}

creatPromise('./1.txt')
.then(function(res) {
    console.log(res)
    return creatPromise('./2.txt')
})
.then(res => {
    console.log(res)
    return creatPromise('./3.txt')
})
.then(res => {
    console.log(res)
})
```

