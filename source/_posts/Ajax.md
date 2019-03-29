@[TOC](这里写自定义目录标题)

# Ajax学习

## 1.原生Ajax
```javascript
// 使用Ajax发送请求需要如下几步：

//1.创建XMLHttpRequest对象
var xhr = new XMLHttpRequest;
//2.准备发送
xhr.open('get','./01.check.php?username='+uname+'&password='+pw,true);
//3.执行发送动作
xhr.send(null);
//4.指定回调函数
xhr.onreadstatechane = function(){
  if(xhr.readState == 4){
    if(xhr.status == 200){
      var data = xhr.responseText;
      var info = document.getElementById('info');
      if(data == '1'){
        info.innerHTML = '登录成功';
      }else if(data == '2'){
        info.innerHTML = '用户名或者密码错误';
      }
    }
  }
}
```
**1.创建XMLHttpRequest对象**
```javascript
var xhr = null;
if(window.XMLHttpRequest){
  xhr = new XMLHttpRequest();//标准
}else{
  xhr = new ActiveObject("Microsoft");//IE6
}
```
**2. 准备发送**
- 参数一：请求方式(get获取数据，post提交数据)
- 参数二：请求地址
- 参数三：同步或异步标志位，默认true表示异步，false表示同步

如果是get请求那么请求参数必须在url中传递，**encodeURI()**
用来对中文参数进行编码，防止乱码

```javascript
var param = 'username'+uname+'&password='+pw;
xhr.open('get','03get.php?'+encodeURI(param),true);
```
**3. 执行发送动作**
get请求在这里需要添加null参数，null不能省
```javascript
xhr.send(null);
```
post请求参数通过send传递，不需要通过encodeURI()转码
```javascript
xhr.open('post','04post.php',false);
```
**4. 指定回调函数**
- readState=1表示已经发送了请求
- readState=2表示浏览器已经接收到了服务器响应的数据
- readState=3表示正在解析数据
- readState=4表示数据已经解析完成，可以使用了，但是这个数据不一定是正常的

 _HTTP常见的状态码_：
    200：响应成功
    404：没有找到请求的资源
    500：服务器端错误


## 2.Json

json数据和普通js对象的区别：
1. json数据没有变量
2. json行驶的数据结尾没有分号
3. json数据中的链必须用双引号包住

```javascript
var str = '{"name":"zahngsan","age":"12"}';
var obj = JSON.parse(str);//把json行驶的字符串转换成对象

var str1 = JSON.stringify(obj);//把对象转成字符串


window.onload = function() {
  var btn = document.getElementById('btn');
  btn.onclick = function(){
    var uname = document.getElementById('username').value;
    var pw = document.getElementById('password').value;

    //1.创建XMLHttpRequest对象
    var xhr = null;
    if(window.XMLHttpRequest){
      xhr = XMLHttpRequest();
    }else{
      xhr = ActiveObject('Microsoft');
    }
    //2.准备发送
    var param = 'username='+uname+'&password='+pw;
    xhr.open('post','07.php',true);
    //3.执行发送动作
    xhr.setRequestHeader('Content-Type',"application/x-www-form-urlencoded");
    xhr.send(param);//post在这里传递，不需要转码
    //4.指定回调函数
    xhr.onreadstatechange = function(){
      if(xhr.readState == 4){
        if(xhr.status == 200){
          var data = xhr.responseText;
          var obj = JSON.parse(data);
          var tag = '<div><span>'+obj.info+'</span><span>-----</span><span>'+obj.message+'</span></div>';
          var info = document.getElementById('info');
          info.innerHTML = tag;
          console.log(2);


          //console.log(data.name1);
          //console.log(data.name2);
        }
      }
    }
    console.log(3);
  }
}

```

## 封装Ajax
```javascript
function ajax(url,type,param,dataType,callback){
  var xhr = null;
  if(window.XHRHttpRequest){
    xhr = new XMLHttpRequest();
  }else{
    xhr = new ActiveXObject('Microsoft.XMLHTTP');
  }
  if(type == 'get'){
    url += "?"+param;
  }
  xhr.open(type,url,true);

  var data = null;
  if(type == 'post'){
    if(type == 'post'){
      data = param;
      xhr.setRequestHeader('Ccontent-Type',"application/x-www-form-urlencoded");
    }
    xhr.send(data);
    xhr.onreadystatechange = function(){
      if(xhr.readyState == 4){
        if(xhr.status == 200){
          var data = xhr.responseText;
          if(dataType == 'json'){
            data = JSON.parse(data);
          }
          callback(data);
        }
      }
    }
  }
}



function ajax(obj){
    // 默认参数
    var defaults = {
        type : 'get',
        data : {},
        url : '#',
        dataType : 'text',
        async : true,
        success : function(data){console.log(data)}
    }
    // 处理形参，传递参数的时候就覆盖默认参数，不传递就使用默认参数
    for(var key in obj){
        defaults[key] = obj[key];
    }
    // 1、创建XMLHttpRequest对象
    var xhr = null;
    if(window.XMLHttpRequest){
        xhr = new XMLHttpRequest();
    }else{
        xhr = new ActiveXObject('Microsoft.XMLHTTP');
    }
    // 把对象形式的参数转化为字符串形式的参数
    /*
    {username:'zhangsan','password':123}
    转换为
    username=zhangsan&password=123
    */
    var param = '';
    for(var attr in obj.data){
        param += attr + '=' + obj.data[attr] + '&';
    }
    if(param){
        param = param.substring(0,param.length - 1);
    }
    // 处理get请求参数并且处理中文乱码问题
    if(defaults.type == 'get'){
        defaults.url += '?' + encodeURI(param);
    }
    // 2、准备发送（设置发送的参数）
    xhr.open(defaults.type,defaults.url,defaults.async);
    // 处理post请求参数并且设置请求头信息（必须设置）
    var data = null;
    if(defaults.type == 'post'){
        data = param;
        xhr.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
    }
    // 3、执行发送动作
    xhr.send(data);
    // 处理同步请求，不会调用回调函数
    if(!defaults.async){
        if(defaults.dataType == 'json'){
            return JSON.parse(xhr.responseText);
        }else{
            return xhr.responseText;
        }
    }
    // 4、指定回调函数（处理服务器响应数据）
    xhr.onreadystatechange = function(){
        if(xhr.readyState == 4){
            if(xhr.status == 200){
                var data = xhr.responseText;
                if(defaults.dataType == 'json'){
                    // data = eval("("+ data +")");
                    data = JSON.parse(data);
                }
                defaults.success(data);
            }
        }
    }

}





调用：
window.onload = function(){
  var btn1 = document.getElementById('btn1');
  btn1.onclick = function(){
    ajax('./10-1.php','get','username=李四&password=123','text',function(data){
      var div1 = document.getElementById('info1');
      div1.innerHTML = data;
    });
  }
  var btn2 = document.getElementById('btn2');
  btn2.onclick = function(){
      ajax('./10-2.php','post','username=张三&password=123456','json',function(data){
          var div2 = document.getElementById('info2');
          // div2.innerHTML = data;
          div2.innerHTML = data.username + "=====" + data.password;
      });
  }
}
```

## jQuer-Ajax相关的API基本使用
```javascript
window.onload = function(){
  var btn = document.getElementById('btn');
  btn.onclick = function(){
    var code = document.getElementById('code').value;
    $.ajax({
      type:'post',
      url:'./11.php',
      data:{code:code},
      dataType:'json',
      success:function(data){
        /*var info = document.getElementByid('info');
        if(data.flag == 0){
          info.innerHTML = "没有这本书";
        }else{
          var tag = '<ul><li>书名：' + data.bookname + '</li><li>作者：' + data.author + '</li><li>描述：' + data.desc + '</li></ul>';
          info.innerHTML = tag;*/
          if(data.flag == 0){
            $('#info').html("该图书不存在");
          }else{
            var tag = '<ul><li>书名：' + data.bookname + '</li><li>作者：' + data.author + '</li><li>描述：' + data.desc + '</li></ul>';
            $("#info").html(tag);
          }
        },
        error:function(data){
          console.log(data);
          $("#info").html("服务器发生错误，请与管理员联系");
        }
      }
    })
  }
}








精简：
var data = ajax({
  type:'post',
  url:'./14.php',
  dataType:'json',
  async:false,
  //data:{username:"张三",password:"123"}
 data:{code:code}
});
/*console.log(data);
var html = '<div><span>用户名：'+data.password+'</span><span>密码：'+data.password+'</span></div>'
$("#info").html(html);*/
data = data.responseJSON;
if(data.flag == 0){
    $("#info").html("该图书不存在");
}else{
    var tag = '<ul><li>书名：' + data.bookname + '</li><li>作者：' + data.author + '</li><li>描述：' + data.desc + '</li></ul>';
    $("#info").html(tag);
}

```
