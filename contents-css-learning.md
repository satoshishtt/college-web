## CSSで画面のデザインをしよう

CSSとはスタイルシートとも呼ばれデザインや見栄えを整える言語

### ①画面カラーと吹き出しの作成
#### 修正するファイル
・public/stylesheets/style.css  
・views/index.ejs  

#### public/stylesheets/style.cssを修正
変更前
```
body {
  padding: 50px;
  font: 14px "Lucida Grande", Helvetica, Arial, sans-serif;
}

a {
  color: #00B7FF;
}
```
↓
変更後
```
body {
  padding: 50px;
  font: 14px "Lucida Grande", Helvetica, Arial, sans-serif;
  background: #d7ebfe;
}

a {
  color: #00B7FF;
}

.serif-left {
  display: block;
  position: relative; 
  margin: 5px 185px 50px 185px;
  padding: 17px 13px;
  border-radius: 12px;
  background: #fbfdfa;
}

.serif-left:after {
  content: "";
  display: inline-block;
  position: absolute;
  top: 18px; 
  left: -24px;
  border: 12px solid transparent;
  border-right: 12px solid #fbfdfa;
}

.serif-right {
  display: block;
  position: relative; 
  margin: 5px 185px 50px 185px;
  padding: 17px 13px;
  border-radius: 12px;
  background: #3cc50e;
}

.serif-right:after {
  content: "";
  display: inline-block;
  position: absolute;
  top: 18px; 
  left: 100%;
  border: 12px solid transparent;
  border-left: 12px solid #3cc50e;
}

.scenery {
  width: 100%;
  margin: 1.5em 0;
  overflow: hidden;
}

```
### views/index.ejsを修正
変更前
```
    <div>
      <p>Hi, I am John. I am your student today. Talk to you soon!</p>
    </div>
```
↓
変更後
```
    <div class="scenery">
      <p class="serif-left">Hi, I am John. I am your student today. Talk to you soon!</p>
    </div>
```
変更前
```
    $('button').on('click', function() {
      var text = $('#message').val();
      $('.scenery').append('<p>' + text + '</p>'); 
      socket.emit('request', text);
    });
    socket.on('response', function(param) {
      $('.scenery').append('<p>' + param + '</p>'); 
    });
```
↓
変更後
```
    $('button').on('click', function() {
      var text = $('#message').val();
      $('.scenery').append('<p class="serif-right">' + text + '</p>'); 
      socket.emit('request', text);
    });
    socket.on('response', function(param) {
      $('.scenery').append('<p class="serif-left">' + param + '</p>'); 
    });
```

### ②画像を表示
#### 修正するファイル
・public/stylesheets/style.css  
・views/index.ejs  
・public/images/left.png *新規追加  
・public/images/right.png *新規追加  

#### public/stylesheets/style.cssを修正
↓
変更後 以下を最後に追加
```
.scenery .faceicon-left {
  float: left;
  margin-right: -90px;
  width: 80px;
}

.scenery .faceicon-right {
  float: right;
  margin-left: -90px;
  width: 80px;
}

.scenery img{
  width: 100%;
  height: 50%;
  height: auto;
  border: solid 3px #d7ebfe;
  border-radius: 50%;
}
```

#### views/index.ejsを修正

変更前
```
    <div class="scenery">
      <p class="serif-left">Hi, I am John. I am your student today. Talk to you soon!</p>
    </div>
```
↓
変更後
```
    <div class="scenery">
        <img class="faceicon-left" border="0" src="/images/left.png">
        <p class="serif-left">Hi, I am John. I am your student today. Talk to you soon!</p>
    </div>
```

変更前
```
    $('button').on('click', function() {
      var text = $('#message').val();
      $('.scenery').append('<p class="serif-right">' + text + '</p>'); 
      socket.emit('request', text);
    });
    socket.on('response', function(param) {
      $('.scenery').append('<p class="serif-left">' + param + '</p>'); 
    });
```
↓
変更後
```
    $('button').on('click', function() {
      var text = $('#message').val();
      $('.scenery').append('<img class="faceicon-right" border="0" src="/images/right.png">'); 
      $('.scenery').append('<p class="serif-right">' + text + '</p>'); 
      socket.emit('request', text);
    });
    socket.on('response', function(param) {
      $('.scenery').append('<img class="faceicon-left" border="0" src="/images/left.png">'); 
      $('.scenery').append('<p class="serif-left">' + param + '</p>'); 
    });
```
  