---
title: 班级同学最相似体型计算-Web版本
date: 2017-11-25
categories: Web
tags: [ajax,JavaScript,json,sqlite]
---

## 思路与说明

 场景续上前三次的场景.

后台通过Python不断爬取同学们的信息, 然后存入数据库. 前端通过ajax访问自己的后台, 然后后台此时读数据库, 并把数据传到前台, 用表格形式呈现. 当需要计算时候, 可以发回后台计算, 得到结果把两人数据标红. (也可以在前端完成计算)

## 代码

因为做的时候已经不在实验室了. 所以数据是自己手动写到数据库的….[拍脸]

这一段是手动写到数据库的命令行版本…真是SQL...

```sql
import sqlite3
conn = sqlite3.connect('student.db')
exit()
sqlite3 student.db
create table Student(ID test, Height int, Weight int, Size text);
insert into Student values(1527406002,172,71,'L');
select * from Student;
.exit

```

后来还是写了一个`db.py` 来把数据写到数据库.

```python
import sqlite3

def insert(id, height, weight, size):
  conn = sqlite3.connect('student.db')
  c = conn.cursor()
  c.execute('insert into Student values(?,?,?,?)',(id,height,weight,size))
  conn.commit()
  conn.close()


if __name__ == '__main__':
  x = [{"id": "1527406002", "height": 172, "weight": 71, "size": "L"},
    {"id":"1509401051","height": 180,"weight":"70","size":"XXXL"},
    {"id": 1527406012, "height": 179, "weight": 84, "size": "XL"},
    {"id": 1527406058, "height": 195, "weight": 80, "size": "XXXL"},
    {"height": 176,"id": 1527406045, "size": "L", "weight": 58},
    {"id": "1527406027", "height": 160, "weight": 75, "size": "L"},
    {"size": "XXXL", "id": "1527406034", "weight": 83, "height": 178},
    {"size": "S", "id": "1527406006", "weight": "45", "height": "157"},
    {"id": "1527406008", "height": 100, "weight": 60, "size": "XL"},
    {"id": 1527406056, "height": 172, "weight": 60, "size": "L"},
    {"size": "L", "id": "1527406032", "weight": 70, "height": 180},
    {"id": 1527406048, "size": "M", "weight": 50, "height": 160},
    {"size": "M", "id": 1527406022, "weight": 49, "height": 159}]
  for i in x:
    insert(**i)
```

然后后台的python是:

```python
#coding:utf-8
from flask import Flask, send_file
import json
import sqlite3
app = Flask(__name__)

@app.route('/', methods=['GET'])
def stu():
  conn = sqlite3.connect("student.db")
  c = conn.cursor()
  c.execute('select * from Student')
  x = c.fetchall()
  conn.commit()
  conn.close()
  #zip: 一一对应合并成list
  y = list(map(lambda i:dict(zip(('id','height','weight','size'),i)), x))
  return json.dumps(y)

@app.route('/index',methods=['GET'])
def index():
  return send_file("index.html")

@app.route('/style.css', methods=['GET'])
def style():
  return send_file("styles.css")

@app.route('/scripts.js', methods=['GET'])
def scripts():
  return send_file("scripts.js")

@app.route('/favicon.ico',methods=['GET'])
def ico():
  return send_file("favicon.ico")


if __name__ == '__main__':
  app.run(debug=True)

```

前端的三个代码如下:

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<!-- initial-scale = 0.65 interesting -->
<meta content="width=device-width, initial-scale=0.65" name="viewport">
<title>相似体型计算</title>
<link rel="stylesheet" href="https://apps.bdimg.com/libs/jquerymobile/1.4.5/jquery.mobile-1.4.5.min.css">
<link rel="stylesheet" href="style.css">
<script src="https://apps.bdimg.com/libs/jquery/1.10.2/jquery.min.js"></script>
<script src="https://apps.bdimg.com/libs/jquerymobile/1.4.5/jquery.mobile-1.4.5.min.js"></script>
</head>
<body>

<div data-role="page" id="pageone">
  <div data-role="header">
    <h1>最相似体型的计算</h1>
  </div>

  <div data-role="main" class="ui-content">
    <table data-role="table" class="ui-responsive ui-shadow" id="Table1">
      <thead>
        <tr>
          <th>ID</th>
          <th>Height</th>
          <th>Weight</th>
          <th>Size</th>
        </tr>
      </thead>
      <tbody id="tbody">

      </tbody>
    </table>

    <div data-role="controlgroup" data-type="horizontal">
      <button id="get_info">获取信息</button>
      <button id="compute">计算结果</button>
    </div>
  </div>

  <div data-role="footer">
    <h1>Welcome</h1>
  </div>

<p>只是一段有意思的内容 来自网络 点击可刷新</p>
<div data-role="controlgroup" data-type="horizontal">
  <button id="hitokoto" onclick=getHitokoto()>一言</button>
</div>
<blockquote id="quote"></blockquote>
</div>

<script src="scripts.js"></script>
</body>
</html>
```

CSS:

```css
blockquote { border-left: .1em solid gray; padding-left: .2em;  }
tr{
  border-bottom: 1px solid #d6d6d6;
}
tr:nth-child(even){
  background: #e9e9e9;
}
thead{
  background: #e9e9e9;
}


/*@media screen and (max-width: 500px) {
  table {
    zoom: 0.5;
    max-width: 100%;
    width: 100%;
  }
}
*/
```

JS:

```javascript
stu = null;
res = null;

// function distance(a,b){
//   return Math.hypot(a.height - b.height, a.weight-b.weight, a.size-b.size);
// }

const distance = (a,b) => Math.hypot(a.height - b.height, a.weight-b.weight, a.size-b.size);
// hypot 计算距离

$(document).on("pagecreate","#pageone",function(){
  $("#get_info").on("click", function(e){
    stu = [];
    $.ajax({
      type: "get",
      dataType: "json",
      url: "http://127.0.0.1:5000/",
      success: function(msg){
        var str="";
        for(i in msg){
          //id = "s"+ msg[i].id +"\"
          str += "<tr id=\"s"+ msg[i].id +"\"><td>" + msg[i].id + "</td><td>" + msg[i].height + "</td><td>" + msg[i].weight + "</td><td>" + msg[i].size + "</td></tr>";
          $("#tbody").append(str);
          str = "";
          msg[i].size = ['S','M','L','XL','XXL','XXXL'].indexOf(msg[i].size.toUpperCase());
          stu.push(msg[i]);
        }
      }
    });
        //Remove an event handler.
        $("#get_info").off();
      });
    $("#compute").on("click", function(){
      if(!stu)
        return ;
      var min = 1/0;
      for (ai in stu) {
        for (bi in stu) {
          if (ai >= bi)
            continue;
          var a = stu[ai];
          var b = stu[bi];
          if (min > distance(a, b)){
            min = distance(a,b);
            res = [a,b];
          }
        }
      }
      //console.log(res);
      //
      res.forEach(e => {
        //jquery .css
        $('#s' + e.id).css('color', 'red');
      });
    });
});

function getHitokoto(){
  fetch('https://sslapi.hitokoto.cn/?c=d&encode=text').then(x => x.text()).then(x => document.getElementById('quote').innerHTML = x)
}

window.onload = function(){
  getHitokoto();
};


```



