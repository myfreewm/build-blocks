
#express-form 

1. npm install multiparty --save


``` javascript

var express = require('express');
var router = express.Router();
var multiparty = require('multiparty');
var util = require('util');
var fs = require('fs');


/* GET home page. */
router.get('/', function(req, res, next) {
  res.render('index', { title: 'Express' });
});

/* 上传*/
router.post('/file/uploading', function(req, res, next){
//生成multiparty对象，并配置上传目标路径
    var form = new multiparty.Form({uploadDir: './tmn/'});
    //上传完成后处理
     form.parse(req, function(err, fields, files) {

         var filesTmp = JSON.stringify(files,null,2);
         console.log(fields); 
         if(err){
            console.log('parse error: ' + err);
        }else {
             console.log('parse files: ' + filesTmp);
             var inputFile = files.inputFile[0];
             var uploadedPath = inputFile.path;
             var dstPath = './tmn/' + inputFile.originalFilename;
             //重命名为真实文件名
             fs.rename(uploadedPath, dstPath, function(err) {
                 if(err){
                    console.log('rename error: ' + err);
                 } else {
                    console.log('rename ok');
                 }
            });
        }

        res.writeHead(200, {'content-type': 'text/plain;charset=utf-8'});
        res.write('received upload:\n\n');
        res.end(util.inspect({fields: fields, files: filesTmp}));
    });
});
module.exports = router;
```
``` html
extends layout

block content
  form(method='post', action='/file/uploading', enctype='multipart/form-data')
    input(name='inputFile', type='file', multiple='mutiple')
    input(name='name', type='text' )
    input(name='btnUp', type='submit',value='上传')

```
