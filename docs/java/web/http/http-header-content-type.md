#### content-type: 期望发送/接收到消息的类型
#### 三种: 
    1、content-type: application/x-www-form-urlencoded
    2、content-type: application/json
    3、content-type: multipart/form-data
#### 三种的区别
##### application/x-www-from-urlencoded
    application/x-www-form-urlencoded: 窗体数据被编码为名称/值对。这是标准的编码格式。
    application/x-www-form-urlencoded 是get/post请求默认的编码格式
    1、当action为get时候，浏览器用x-www-form-urlencoded的编码方式把form数据转换成一个字串（name1=value1&name2=value2…），然后把这个字串append到url后面，用?分割，加载这个新的url。
    2、当action为post时候，浏览器把form数据【键值对】封装到http body中，然后发送到server。如果没有type = file的控件，用默认的application/x-www-form-urlencoded就可以了。

##### application/json
    与application/x-www-from-urlencoded不同的是，浏览器将数据转换成json格式（区别与name1=value1&name2=value2...），传送至后台。

##### multipart/form-data    
    multipart/form-data: 上传文件时才能使用的编码方式。窗体数据被编码为一条消息，页上的每个控件对应消息中的一个部分。 
    浏览器会把整个表单以控件为单位分割，并为每个部分加上Content-Disposition(form-data或者file), Content-Type(默认为text/plain), name(控件name)等信息，并加上分割符(boundary).

#### text/plain（没有碰到过这种情况） 
    text/plain: 窗体数据以纯文本形式进行编码，其中不含任何控件或格式字符。补充
    
##### application/x-www-from-urlencoded 与 application/json的区别
    1、编码方式不同（上面介绍过）
    2、后台接收方式
    application/json：代表参数以【json字符串】传递给后台，controller接收需要@RequestBody 接收参数，只能接受一个对象
        例如：@RequestBody Map<String, Object> map、@RequestBody User user
    application/x-www-form-urlencoded：代表参数以【键值对】传递给后台，controller接收可以单个参数接收、可以对象接收，也可以混合接收
        例如：@RequestParam("param") String param、类接收、混合接收、map接收