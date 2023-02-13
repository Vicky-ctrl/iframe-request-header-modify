# iframe-request-header-modify
这篇文章将分享：应用在Vue项目中，修改<iframe>标签请求头的方法
This article will share: Applied in the Vue project, the method of modifying the request header of the &lt;iframe> tag.

## 我们要做什么？
使用 <iframe> 预览服务器的 .pdf 文件，并且请求文件时携带 token 以供后端验证

## 遇到的问题
官方文档（）中可以改变多个属性，但不能更改默认的 document 请求头

    <iframe id="iframe-page" src="yoururl/pdf" width="100%"></iframe>//常规写法

## 解决办法
在项目中使用<iframe>标签时，可以指定 src 一个 url，也就是我们的请求路径，在我的项目中是返回文件在服务器中的位置

template 部分：

    <iframe id="iframe-page" :src="page_url" width="100%"></iframe>
    
data 部分：

    page_url: null,
    
methods 部分：
    
    makePdf(url){
      this.loadingTask = ''
      this.$http.get(url)
        .then((res) => {
          const { data, status } = res;
          let binaryData = []
          binaryData.push(data)// res 后台返回的流数据
          let pdfUrl = ''
          pdfUrl = window.URL.createObjectURL(new Blob(binaryData, { type: 'application/pdf' }))
          this.page_url = pdfUrl
        })
    },
    
axios 部分：

    // Add a request interceptor
    //添加请求的拦截器
    instance.interceptors.request.use(function(config) {
      if (config.url.indexOf('yoururl/pdf') !== -1) {
        config.responseType = 'blob'
      }
    })
    
    
    
