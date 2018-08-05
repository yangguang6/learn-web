# 1. 闭包
  * ## 概念：
    1. 闭包就是能够读取其他函数内部变量的函数。
    2. 闭包就是将函数内部和函数外部连接起来的一座桥梁。
    3. 可以把闭包简单理解成"定义在一个函数内部的函数"。

  * ## 例子：
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

  * ## 闭包的使用场景：
    ### 1. 函数作为返回值:
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

    ### 2. 函数作为参数传递:
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

  * ## 如何理解作用域：
    1. 自由变量
    2. 作用域链，即自由变量的查找
    3. 闭包的两个场景

# 2. 同步和异步
  * ## 例子：
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

  * ## 同步和异步的区别：
    1. 同步会阻塞代码执行，而异步不会
    2. alert是同步，setTimeout是异步

  * ## 使用异步的场景：
    1. 定时任务：setTimeout, setInterval
    2. 网络请求：ajax请求， 动态`<img>`加载（都需要等待）
    3. 事件绑定

  * ## 除用户输入以外，异步编程技术的三个主要使用场景:
    网络请求(如Ajax请求)、 文件系统操作(读/写文件等)、 刻意的时间延迟功能(比如警告)

# 3. 事件代理/委托
  * ## 编写一个通用的事件监听函数：
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

# 4. ajax
  * ## 例子：
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

  * ## xhr取得响应：
    - responseText: 获得字符串型式的响应数据
    - responseXML: 获得XML形式的响应数据
    - status和statusText: 以数字和文本形式返回HTTP状态码
    - getAllResponseHeader(): 获取所有的响应包头
    - getResponseHeader(): 查询响应中的某个字段的值

  * ## readyState 属性：
    - 0： 请求未初始化， open还没有调用
    - 1： 服务器连接已建立， open已经调用了
    - 2： 请求已接受，也就是接收到头信息了
    - 3： 请求处理中，也就是接收到响应主体了
    - 4： 请求已完成，且响应已就绪，也就是响应完成了

  * ## promise版：
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

# 5. 类型
  * ## Javascript中，只有基本类型和对象这两种值。（按存储方式区分变量类型： 值类型、引用类型）
    ```javascript
      //引用类型例子
      var a = {age: 20}
      var b = a
      b.age = 21
      console.log(a.age)  //21
    ```

  * ## 特殊的数值NaN与任何值都不相等，包括他自己。 (即，NaN === NaN 和 NaN == NaN 都是false)

  * ## 下面的值在JS中都代表false：
    undefined、 null、 false、 0、 NaN、 ''(空字符串)

  * ## 强制类型转换情况：
    1. 字符串拼接
    2. == 运算符
    3. if 语句
    4. 逻辑运算

  * ## 判断一个变量会被当作 true 还是 false :
    ```javascript
    var a = 100
    console.log(!!a)
    ```

  * ## 何时使用 === 何时使用 ==：
    ```javascript
    if (obj.a == null) {
      //这里相当于 obj.a === null || obj.a === undefined  简写形式
    }
    ```

  * ## js中使用 typeof 能够得到哪些类型:
    ```javascript
    typeof undefined  // undefined
    typeof 'abc'  // string
    typeof true  // boolean
    typeof {} // object
    typeof [] // object
    typeof null // object
    typeof console.log // function    
    ```

  * ## 如何准确判断一个变量是数组类型:
    ```javascript
    var arr = []
    arr instanceof Array  //true
    typeof arr  // object, typeof是无法判断是否是数组的
    ```
    instanceof 用于判断**引用类型**属于哪个**构造函数**的方法

# 6. JSON
  * ## 如何理解JSON：
    - JSON 是一个js对象  一种数据类型
    - JSON 是用于存储和传输数据的格式
    - JSON 通常用于服务端向网页传递数据

    ```javascript
    JSON.stringify({a:10, b:20})
    JSON.parse('{"a":10,"b":20}')
    ```

# 7. 原型及原型链
  * ## 原型规则：
    1. 所有的引用类型（数组、对象、函数），都具有对象特性，即可自由扩展属性（除了"null"意外）
    2. 所有的引用类型（数组、对象、函数），都有一个__proto__（隐式原型）属性，属性值是一个普通的对象
    3. 所有的函数，都有一个prototype（显式原型）属性，属性值也是一个普通对象
    4. 所有的引用类型（数组、对象、函数），__proto__属性值指向它的构造函数的"prototype"属性值
    5. 当试图得到一个对象的某个属性时，如果这个对象本身没有这个属性，那么会去它的__proto__（即它的构造函数的prototype）中寻找

  * ## 示例：
    ```javascript
    var obj = {}; obj.a = 100;
    var arr = []; arr.a = 100;
    function fn() {}
    fn.a = 100;

    console.log(obj.__proto__);
    console.log(arr.__proto__);
    console.log(fn.__proto__);

    console.log(fn.prototype);

    console.log(obj.__proto__ === Objcet.prototype)


    //构造函数
    function Foo(name, age) {
      this.name = name
    }
    Foo.prototype.alertName = function () {
      alert(this.name)
    }
    //创建示例
    var f = new Foo('zhangsan')
    f.printName = function () {
      console.log(this.name)
    }
    //测试
    f.printName()
    f.alertName()
    f.toString()  //要去 f.__proto__.__proto__中查找

    var item
    for (item in f) {
      if (f.hasOwnProperty(item)) {
        console.log(item)
      }
    }
    ```

  * ## 写一个贴近实际开发原型链继承的例子:
    ```javascript
    //写一个封装DOM查询的例子
    function Elem(id) {
      this.elem = document.getElementById(id)
    }

    Elem.prototype.html = function (val) {
      var elem = this.elem
      if (val) {
        elem.innerHTML = val
        return this  //链式操作
      } else {
        return elem.innerHTML
      }
    }

    Elem.prototype.on = function (type, fn) {
      var elem = this.elem
      elem.addEventListener(type, fn)
      return this  //返回对象
    }

    var div1 = new Elem('detail-page')
    // console.log(div1.html())
    div1.html('<p>hello imooc</p>').on('click', function () {
      alert('clicked')
    }).html('<p>javascript</p>')
    ```

# 8. this
  * ## 在javascript中, this是动态绑定的，或称为运行期绑定的，所以导致javascript中的this关键字有能力具备多重含义。由于其运行期绑定的特性，javascript中的this含义要丰富得多，他可以是全局对象、当前对象或者任意对象，这完全取决于函数的调用方式。
  
  * ## this不同的使用场景：
    1. 作为构造函数执行
    2. 作为对象属性执行
    3. 作为普通函数执行
    4. call apply bind

  * ## 例子：
    ```javascript
    //作为构造函数调用
    function Foo(name){
        console.log(this);    //先 this = {}  最后 return this
        this.name = name;
    }
    var f = new Foo('zhangsan')

    //作为对象属性调用
    var obj = {
      name: 'A',
      printName: function(){
        console.log(this.name)
      }
    }
    obj.printName()

    //作为普通函数调用
    function fn(){
      console.log(this)
    }
    fn()   //this === window

    //函数被调用时，this被绑定到全局对象，接下来执行赋值语句，相当于隐式声明了一个全局变量，这是非常危险的，很容易造成内存泄漏。
    function makeNoSense(x) {    
      this.x = x;
    }
    makeNoSense(5);
    x;    //x已经成为一个值为5的全局变量


    //使用call apply bind调用
    function fn1(name){
      alert(name)
      console.log(this)
    }
    fn1.call({x:100}, 'zhangsan')  //this为第一个参数
    ```

  * ## 描述 new 一个对象的过程:
    - ### 简述版：
      1. 创建一个新对象
      2. this指向这个新对象
      3. 执行代码，即对this赋值
      4. 返回this
    
    - ### 详述版：
      1.创建一个空对象
      `var obj = new Objcet();`
      2.让Foo中的this指向obj，并执行Foo的函数体
      `var result = Foo.call(obj);`
      3.设置原型链，将obj的__proto__成员指向了Foo函数对象的prototype成员对象
      `obj.__proto__ = Foo.prototype;`
      4.判断Foo的返回值类型，如果是值类型，返回obj。如果是引用类型，就返回这个引用类型的对象。
      ```
      if(typeof(result) == "object")
        f = result;
      else
        f = obj;
      ```

  * ## 函数的执行环境:
    JavaScript 中的函数既可以被当作普通函数执行，也可以作为对象的方法执行，这是导致 this 含义如此丰富的主要原因。一个函数被执行时，会创建一个执行环境（ExecutionContext），函数的所有的行为均发生在此执行环境中，构建该执行环境时，JavaScript 首先会创建 arguments变量，其中包含调用函数时传入的参数。接下来创建作用域链。然后初始化变量，首先初始化函数的形参表，值为 arguments变量中对应的值，如果 arguments变量中没有对应值，则该形参初始化为 undefined。如果该函数中含有内部函数，则初始化这些内部函数。如果没有，继续初始化该函数内定义的局部变量，需要注意的是此时这些变量初始化为 undefined，其赋值操作在执行环境（ExecutionContext）创建成功后，函数执行时才会执行，这点对于我们理解 JavaScript 中的变量作用域非常重要，鉴于篇幅，我们先不在这里讨论这个话题。最后为 this变量赋值，如前所述，会根据函数调用方式的不同，赋给 this全局对象，当前对象等。至此函数的执行环境（ExecutionContext）创建成功，函数开始逐行执行，所需变量均从之前构建好的执行环境（ExecutionContext）中读取。

# 9. 作用域
  * ## 示例：
    ```javascript
    //无块级作用域
    if(true){
      var name = 'zhangsan'
    }
    console.log(name)

    //函数和全局作用域
    var a = 100
    function fn(){
      var a = 200
      console.log('fn', a)
    }
    console.log('global', a)
    fn()
    ```

# 10. 日期和math：
  * ## 示例：
    ```javascript
    //日期
    Date.now()    //获取当前时间毫秒数
    var dt = new Date()
    dt.getTime()    //获取毫秒数
    dt.getFullYear()
    dt.getMonth()   // 0-11
    dt.getDate()    // 0-31
    dt.getHours()   // 0-23
    dt.getMinutes() // 0-59
    dt.getSeconds() // 0-59

    //Math
    获取随机数 Math.random()

    //获取随机数，要求是长度一致的字符串
    var random = Math.random()
    random = random + '0000000000'   //后面加上10个0
    random = random.slice(0, 10)
    console.log(random)
    ```

# 11. 数组和对象的 API
  * ## 数组API：
    ### 1. forEach 遍历所有元素
      ```javascript
      var arr = [1,2,3]
      arr.forEach(function(item, index){
        //遍历数组的所有元素
        console.log(index, item)
      })
      ```
    ### 2. every 判断所有元素是否都符合条件
      ```javascript
      var arr = [1,2,3]
      var result = arr.every(function(item,index){
        //用来判断所有的数组元素，都满足一个条件
        if(item < 4){
          return true
        }
      })
      console.log(result)
      ```  
    ### 3. some 判断是否有至少一个元素符合条件
      ```javascript
      var arr = [1,2,3]
      var result = arr.some(function(item, index){
        //用来判断所有的数组元素，只要有一个满足条件即可
        if(item < 2){
          return true
        }
      })
      console.log(result)
      ```

    ### 4. sort 排序
      ```javascript
      var arr = [1,4,2,3,5]
      var arr2 = arr.sort(function(a, b){
        //从小到大排序
        return a - b 

        //从大到小排序
        //return b - a
      })
      console.log(arr2)
      ```

    ### 5. map 对元素重新组装，生成新数组
      ```javascript
      var arr = [1,2,3,4]
      var arr2 = arr.map(function(item, index){
        //将元素重新组装，并返回
        return '<b>' + item + '</b>'
      })
      console.log(arr2)
      ```

    ### 6. filter 过滤符合条件的元素
      ```javascript
      var arr = [1,2,3]
      var arr2 = arr.filter(function(item, index){
        //通过某一个条件过滤数组
        if(item >= 2){
          return true
        }
      })
      console.log(arr2)
      ```

  * ## 对象API：
    ```javascript
    var obj = {
      x: 100,
      y: 200,
      z: 300
    }
    var key 
    for(key in obj){
      if(obj.hasOwnProperty(key)){
        console.log(key, obj[key])
      }
    }
    ```

  * ## 写一个能遍历对象和数组的forEach函数:
    ```javascript
    function forEach(obj, fn){
      var key
      if(obj instanceof Array){
        //准确判断是不是数组
        obj.forEach(function(item,index){
          fn(index, item)
        })
      }else{
        //不是数组就是对象
        for(key in obj){
          fn(key, obj[key])
        }
      }
    }
    var arr = [1,2,3]
    //注意，这里参数的顺序换了，为了和对象的遍历格式一致
    forEach(arr, function(index, item){
      console.log(index, item)
    })
    var obj = {x: 100, y: 200}
    forEach(obj, function(key, value){
      console.log(key, value)
    })
    ```

  * ## 去除数组重复成员的方法：
    1. `[...new Set(array)]`
    2. Array.from方法可以将 Set 结构转为数组。
        ```javascript
        function dedupe(array) {
          return Array.from(new Set(array));
          }
          
        dedupe([1, 1, 2, 3]) // [1, 2, 3]
        ```

# 12. BOM 操作
  * 示例：
    ```javascript
    //navigator
    var ua = navigator.userAgent
    var isChrome = ua.indexOf('Chrome')
    console.log(isChrome)

    //screen
    console.log(screen.width)
    console.log(screen.height)

    //location
    console.log(location.href)
    console.log(location.protocol)  //'http:' 'https:'
    console.log(location.pathname)  // '/learn/199'
    console.log(location.search)
    console.log(location.hash)

    //history
    history.back()
    history.forward()
    ```

# 13. 跨域
  * 可以跨域的三个标签：
    - `<img src = xxx>`  用于打点统计，统计网站可能是其他域
    - `<link href = xxx>`  可以使用CDN
    - `<script src = xxx>`  可以使用CDN 可以用于JSONP

# 14. 性能优化
  * ## 原则：
    1. 多使用内存、缓存或者其他方法
    2. 减少CPU计算、减少网络

  * ## 加载资源优化：
    1. 静态资源的压缩合并
    2. 静态资源缓存
    3. 使用CDN让资源加载更快
    4. 使用SSR后端渲染，数据直接输出到HTML中

  * ## 渲染优化：
    1. CSS放前面，JS放后面
    2. 懒加载（图片懒加载、下拉加载更多）
    3. 减少DOM查询，对DOM查询做缓存
    4. 减少DOM操作，多个操作尽量合并在一起执行
    5. 事件节流
    6. 尽早执行操作（ 如DOMContentLoaded ）