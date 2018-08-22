# nodeJs 让你的本地项目跑起来

```
index.js

var http = require('http');
var fs = require('fs');//引入文件读取模块
var path=require('path');
var documentRoot = 'C:/Users/yinfangli/Desktop/companyProject/nbi';
//需要访问的文件的存放目录（项目所在位置的文件夹路径）
const types = {
  "css": "text/css",
  "gif": "image/gif",
  "html": "text/html",
  "ico": "image/x-icon",
  "jpeg": "image/jpeg",
  "jpg": "image/jpeg",
  "js": "text/javascript",
  "json": "application/json",
  "pdf": "application/pdf",
  "png": "image/png",
  "svg": "image/svg+xml",
  "swf": "application/x-shockwave-flash",
  "tiff": "image/tiff",
  "txt": "text/plain",
  "wav": "audio/x-wav",
  "wma": "audio/x-ms-wma",
  "wmv": "video/x-ms-wmv",
  "xml": "text/xml"
}

var server= http.createServer(function(req,res){

    var url = req.url; 
    //客户端输入的url，例如如果输入localhost:8888/index.html
    //那么这里的url == /index.html 

    var file = documentRoot + url;
    console.log(url);
    var ext = path.extname(file);
    console.log(ext, '11111')
    ext = ext ? ext.slice(1) : 'unknown';
    
    //E:/PhpProject/html5/websocket/www/index.html 


    fs.readFile( file , function(err,data){
    /*
        一参为文件路径
        二参为回调函数
            回调函数的一参为读取错误返回的信息，返回空就没有错误
            二参为读取成功返回的文本内容
    */
        if(err){
            res.writeHeader(404,{
                'content-type' : 'text/html;charset="utf-8"'
            });
            res.write('<h1>404错误</h1><p>你要找的页面不存在</p>');
            res.end();
        }else{
            res.writeHeader(200,{
                'content-type' : types[ext] || "text/plain"
            });
            res.write(data);//将index.html显示在客户端
            res.end();

        }

    });



}).listen(8081);

console.log('服务器开启成功');


cmd
cd node
node index.js

```