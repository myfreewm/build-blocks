
# body-parser

## body-parser 是一个对post请求的请求体进行解析的express中间件

``` javascript
/**
 *测试bodyparser如何使用 
 *
 *
 * */
var express = require('express');
var bodyParser = require('body-parser');

var path = require('path');

var app = express();


app.use(bodyParser.json());
app.use(bodyParser.urlencoded({extended:false}));

app.get('/',function(req,res){

    res.send('<form method="POST" action="./form">\
        <input type="text" name="user">\
        <input type="submit" value="submit">\
        </form>');
});

app.post('/form',function(req, res){

    console.log(req.body);
    var user = req.body.user;
    res.send('账号：'+ user);

});

app.listen(3000);



```

