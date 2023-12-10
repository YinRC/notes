# 1. ajax 简介

## 1.1 ajax 本义

ajax = asyn javascript and xml 就是==异步的 js和 xml==

可以实现无刷新地获取数据

## 1.2 ajax 优缺点

**优点：**

+ 可以在不刷新的情况下与服务端进行通信
+ 允许根据用户事件来更新部分网页内容

**缺点：**

+ 没有浏览历史，不能回退
+ 存在跨域问题（同源）
  + a.com 访问不到 b.com
+ SEO 不友好
  + 搜索引擎优化不友好
  + 爬虫爬不到因为是动态创建的

# 2. http 协议

## 2.1 请求报文

![image-20231210190424247](./ajax.assets/image-20231210190424247.png)

（请求）行：请求类型GET/POST、url、协议版本HTTP/1.1

（请求）头：Host、Cookie、Content-Type、User-Agent

（*must）空行

（请求）体：**GET请求->请求体为空**、POST请求->请求体可以不为空

## 2.2 响应报文

![image-20231210190957473](./ajax.assets/image-20231210190957473.png)

（响应）行：协议版本HTTP/1.1、响应状态码200、响应字符串OK

（响应）头：Content-Type、Content-Length、Content-encoding

（*must）空行

（响应）体：<html></html>

# 3. nodejs express 演示

## 3.1 准备工作

![image-20231210192350166](./ajax.assets/image-20231210192350166.png)

![image-20231210192630481](./ajax.assets/image-20231210192630481.png)

## 3.2 实际操作

### 3.2.1 GET 请求

![image-20231210193302864](./ajax.assets/image-20231210193302864.png)

![image-20231210193624593](./ajax.assets/image-20231210193624593.png)

![image-20231210193916005](./ajax.assets/image-20231210193916005.png)

![image-20231210193940664](./ajax.assets/image-20231210193940664.png)

### 3.2.2 POST 请求

![image-20231210194703148](./ajax.assets/image-20231210194703148.png)

![image-20231210195445107](./ajax.assets/image-20231210195445107.png)

![image-20231210195710508](./ajax.assets/image-20231210195710508.png)

![截屏2023-12-10 20.00.21](./ajax.assets/%E6%88%AA%E5%B1%8F2023-12-10%2020.00.21.png)

![image-20231210201008228](./ajax.assets/image-20231210201008228.png)

![image-20231210201140476](./ajax.assets/image-20231210201140476.png)

![image-20231210201208542](./ajax.assets/image-20231210201208542.png)

![image-20231210201545188](./ajax.assets/image-20231210201545188.png)



# 4. IE 缓存问题

![image-20231210202526554](./ajax.assets/image-20231210202526554.png)

# 5. 超时和异常

+ 超时：setTimeout(() => {response.send('超时的响应')}, 3000)
+ 异常：chrome 调试工具online 可设置为offline

![image-20231210203443063](./ajax.assets/image-20231210203443063.png)

# 6. 取消请求

![image-20231210204014036](./ajax.assets/image-20231210204014036.png)

![image-20231210204133127](./ajax.assets/image-20231210204133127.png)

![image-20231210204236450](./ajax.assets/image-20231210204236450.png)

# 7. 重复请求问题

![image-20231210205104276](./ajax.assets/image-20231210205104276.png)

# 8. axios 发送ajax 请求

## 8.1 axios get 请求 axios.get

![image-20231210205711943](./ajax.assets/image-20231210205711943.png)

## 8.2 axios post 请求 axios.post

![image-20231210210219137](./ajax.assets/image-20231210210219137.png)

## 8.3 通用方式发送请求

![image-20231210210850456](./ajax.assets/image-20231210210850456.png)

![image-20231210211035651](./ajax.assets/image-20231210211035651.png)

# 9. 跨域问题

## 9.1 同源策略

+ Same-Origin Policy 是浏览器的一种安全策略

+ 同源：协议、域名、端口

![image-20231210212121400](./ajax.assets/image-20231210212121400.png)

## 9.2 跨域请求 CORS

+ Cross-Origin Resource Sharing 跨域资源共享
+ 不需要在客户端进行任何特殊操作，只需要在服务端进行处理
+ 通过设置响应头来实现，官方推荐

> 通过设置一个响应头来告诉浏览器：该请求允许跨域
>
> 浏览器收到该响应后就会对响应放行

![image-20231210213241534](./ajax.assets/image-20231210213241534.png)

