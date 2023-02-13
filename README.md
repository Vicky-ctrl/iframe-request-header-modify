# iframe-request-header-modify
This article will share: Applied in the Vue project, the method of modifying the request header of the &lt;iframe> tag.

## What are we going to do?
Use <iframe> to preview the .pdf file of the server, and carry the token for backend verification when requesting the file.

## Problem
Multiple attributes can be changed in the [MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe) document, but the default request header cannot be changed, that is, the requirement of passing token in the request header cannot be met.

    <iframe id="iframe-page" src="yoururl/pdf" width="100%"></iframe>//common

## Solution
First, add a judgment on the file path to the request interceptor (just take the public part for judgment), and change the responseType to 'blob'. Next, change the value of src in the page code, that is, parse the file stream returned by the request and assign it to src.

template:

    <iframe id="iframe-page" :src="page_url" width="100%"></iframe>
    
data:

    page_url: null,
    
methods:
    
    makePdf(url){
      this.$http.get(url)
        .then((res) => {
          const { data, status } = res;
          let binaryData = []
          binaryData.push(data)
          let pdfUrl = ''
          pdfUrl = window.URL.createObjectURL(new Blob(binaryData, { type: 'application/pdf' }))
          this.page_url = pdfUrl
        })
    },
    
axios：

    // Add a request interceptor
    instance.interceptors.request.use(function(config) {
      if (config.url.indexOf('yoururl/pdf') !== -1) {
        config.responseType = 'blob'
      }
    })
    
    //over
