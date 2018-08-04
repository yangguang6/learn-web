## 1. 闭包
  * #### 概念：
    闭包就是能够读取其他函数内部变量的函数。
    闭包就是将函数内部和函数外部连接起来的一座桥梁。
    可以把闭包简单理解成"定义在一个函数内部的函数"。

  * #### 例子：
    ```javascript
    function isFirstLoad(){
      var _list = []
      return function(id){
        if(_list.indexOf(id) >= 0){
          return false
        }else{
          _list.push(id)
          return true
        }
      } 
    }
    
    //使用
    var firstLoad = isFirstLoad()
    firstLoad(10) //true
    firstLoad(10) // false
    firstLoad(20) // true
    ```

  * #### 闭包的使用场景：
    ##### 1.函数作为返回值:
    ```javascript
    function F1(){
      var a = 100
      return	function(){
        console.log(a)
      }
    }
    var f1 = F1()
    var a = 200
    f1()    //100
    ```

    ##### 2.函数作为参数传递:
    ```javascript
    function F1(){
      var a = 100
      return	function(){
        console.log(a)  //自由变量，父作用域寻找
      }
    }
    var f1 = F1()

    function F2(fn) {
      var a = 200
      fn()
    }
    F2(f1)  //100
    ```

  * #### 如何理解作用域：
    1.自由变量
    2.作用域链，即自由变量的查找
    3.闭包的两个场景

## 2. 同步和异步
  * #### 例子：
    ```javascript
    //异步
    console.log(100)
    setTimeout(function(){
      console.log(200)
    }, 1000)
    console.log(300)  // 100 300 200

    //同步
    console.log(100)
    alert(200)    //1秒钟后点击确认
    console.log(300)
    ```

  * #### 同步和异步的区别：
    1.同步会阻塞代码执行，而异步不会
    2.alert是同步，setTimeout是异步

  * #### 使用异步的场景：
    1.定时任务：setTimeout, setInterval
    2.网络请求：ajax请求， 动态`<img>`加载（都需要等待）
    3.事件绑定

  * #### 除用户输入以外，异步编程技术的三个主要使用场景:
    网络请求(如Ajax请求)、 文件系统操作(读/写文件等)、 刻意的时间延迟功能(比如警告)

## 3. 事件代理/委托
  * #### 编写一个通用的事件监听函数：
    ```html
    <!DOCTYPE html>
    <html>
      <head>
        <title></title>
        <meta charset="utf-8">
      </head>
      <body>
        <div id="div1">
          <a href="http://baidu.com" id="link1">baidu</a>
          <a href="http://baidu.com" id="link2">baidu</a>
          <p>baidu</p>
          <a href="http://baidu.com" id="link3">baidu</a>
          <a href="http://baidu.com" id="link4">baidu</a>
          <p id="p1">激活</p>
          <p id="p2">取消</p>
        </div>
        <div id="div2">
          <p id="p3">取消</p>
          <p id="p4">取消</p>
        </div>

        <script type="text/javascript">
          function bindEvent(elem, type, selector, fn){
            console.log(fn)
            if (fn == null) {  //这里要使用==  而不是=== ？  或者 fn === undefined  没有fn时其值为undefined
              fn = selector
              selector =null
            }
            elem.addEventListener(type, function(e){
              var target
              if (selector) {
                // 代理
                target = e.target
                // console.log(target)
                if (target.matches(selector)) {
                  fn.call(target, e)
                }
              } else {
                //不是代理
                fn(e)
              }
            })
          }

          var div1 = document.getElementById('div1')
          bindEvent(div1, 'click', 'a', function(e){
          e.preventDefault()
          console.log(this.innerHTML)
          })

          var p1 = document.getElementById('p1')
          bindEvent(p1, 'click', function(e){
          console.log(p1.innerHTML)
          })

          // var div1 = document.getElementById('div1')
          // bindEvent(div1, 'click', function(e){
          //   e.preventDefault()
          //   var target = e.target
          //   if (target.nodeName === 'A') {
          //     console.log('clicked')
          //   }
          // })

          // var p1 = document.getElementById('p1')
          // bindEvent(p1, 'click', function(e){
          //   e.stopPropagation()
          //   alert('激活')
          // })

          // var body = document.body
          // bindEvent(body, 'click', function(e){
          //   alert('取消')
          // })

        </script>
      </body>
    </html>
    ```

## 4. ajax
  * #### 例子：
    ```javascript
    var request = new XMLHttpRequest();  //先实例化一个xhr对象

    //XHR对象的方法：
    open(method,url,async) //发送请求方法，请求地址，请求同步/异步（Ajax是默认值true，可不填写）
    send(string)  //GET可不填参数，POST要填写，否则没意义

    //xhr发送请求方法
    request.open("GET","get.php",true);
    request.send();  

    request.open("POST","create.php",true);
    request.setRequestHeader("Content-type","application/x-www-form-urlencoded");  //一定要写在两者中间
    request.send("name=王二狗&sex=男");


    //例子：
    var request = new XMLHttpRequest();
    request.open("GET","get.php",true);
    request.send();
    request.onreadystatechange = function() {
      if (request.readyState===4 && request.status===200) {
        //做一些事情  request.reponseText
      }
    }
    ```

  * #### xhr取得响应：
    responseText: 获得字符串型式的响应数据
    responseXML: 获得XML形式的响应数据
    status和statusText: 以数字和文本形式返回HTTP状态码
    getAllResponseHeader(): 获取所有的响应包头
    getResponseHeader(): 查询响应中的某个字段的值

  * #### readyState 属性：
    0： 请求未初始化， open还没有调用
    1： 服务器连接已建立， open已经调用了
    2： 请求已接受，也就是接收到头信息了
    3： 请求处理中，也就是接收到响应主体了
    4： 请求已完成，且响应已就绪，也就是响应完成了

  * #### promise版：
    ```javascript
    const getJSON = function (url) {
      const promise = new Promise(function (resolve, reject) {
        const handler = function () {
            if (this.readyState !== 4) {
                return;
            }
            if (this.status === 200) {
                resolve(this.response);
            } else {
                reject(new Error(this.statusText));
            }
        };
        const client = new XMLHttpRequest();
        client.open("GET", url);
        client.onreadystatechange = handler;
        client.responseType = "json";
        client.setRequestHeader("Accept", "application/json");
        client.send();
      });
      return promise;
    };

    getJSON("/posts.json").then(function (json) {
      console.log('Contents: ' + json);
    }, function (error) {
      console.error('出错了', error);
    });
    ```

## 5. 类型
  * ##### Javascript中，只有基本类型和对象这两种值。（按存储方式区分变量类型： 值类型、引用类型）
    ```javascript
      //引用类型例子
      var a = {age: 20}
      var b = a
      b.age = 21
      console.log(a.age)  //21
    ```

  * ##### 特殊的数值NaN与任何值都不相等，包括他自己。 (即，NaN === NaN 和 NaN == NaN 都是false)

  * ##### 下面的值在JS中都代表false：
    undefined、 null、 false、 0、 NaN、 ''(空字符串)

  * ##### 强制类型转换情况：
    1. 字符串拼接
    2. == 运算符
    3. if 语句
    4. 逻辑运算

  * ##### 判断一个变量会被当作 true 还是 false :
    ```javascript
    var a = 100
    console.log(!!a)
    ```

  * ##### 何时使用 === 何时使用 ==：
    ```javascript
    if (obj.a == null) {
      //这里相当于 obj.a === null || obj.a === undefined  简写形式
    }
    ```

## 