### HttpURLConnection 填坑
#### 代理
##### 1、设置主机端口
```
InetSocketAddress addr = new InetSocketAddress(ip, port);
Proxy proxy = new Proxy(Proxy.Type.HTTP, addr);
URLConnection uc = java.net.URL.openConnection(proxy);
```
##### 2、设置用户名密码
```
String password = String.format("%s:%s", proxyUser, proxyPwd);
String encodedPassword = new String(Base64.encodeBase64(new String(password).getBytes()));
// 这里需要了解下 HTTP header消息头
urlConnection.setRequestProperty("Proxy-Authorization", "Basic " + encodedPassword);
```
##### 3、get请求传参

##### 4、post请求传参