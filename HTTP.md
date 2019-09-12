#### HTTP Request Body

HTTP 请求的 body 主要用于提交表单场景。实际上，HTTP 请求的 body 是比较自由的，只要浏览器端发送的 body 服务端认可就可以了。一些常见的 body 格式是：

- application/json
- application/x-www-form-urlencoded
- multipart/form-data
- text/xml

我们使用 HTML 的 form 标签提交产生的 HTML 请求，默认会产生 application/x-www-form-urlencoded 的数据格式，当有文件上传时，则会使用 multipart/form-data。

![](D:\zf7067\Desktop\front end\front-end\assets\HTTP-Body-1.png)

HTTP body 的格式一般定义在 Content-type 中，如上图，body 的格式是 application/json，请求体中的数据也是 json 格式的。



然而，在开发过程中，我曾经遇到这样一个问题：在 POST 请求中，采用 application/json 格式提交数据，后端接收不到（后端用对象接收数据），最后采用 application/x-www-form-urlencoded 格式，即提交表单方式解决了问题。

![](D:\zf7067\Desktop\front end\front-end\assets\HTTP-Body-2.png)