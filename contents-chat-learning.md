## チャットを作ろう
ここからはWEBアプリの中身をプログラミングしてきます。  

### ①リアルタイムWeb機能を実装
［Ctrl］＋［C］ WEBアプリのバッチジョブは停止し、リアルタイムWeb機能のモジュールを追加します。  
```
$ npm i -S socket.io
```

#### VSCdodeで修正
次に「learning」を読み込み、VSCdodeで修正できるようにします。  
1. 「Explorer」を選択する  
2. 「Open Folder」ボタンを押す  
 ![img](./img/contents/vs-2.png "img")  

3. 「learning」フォルダを選択する  
 ![img](./img/contents/vs-fo.png "img")  

4. 「learning」フォルダの中身が表示される  
 ![img](./img/contents/vs-3.png "img")  

#### 修正するファイル
・views/index.ejs  
・route/index.js  
・bin/www
  
#### views/index.ejsを修正
変更前
```
<!DOCTYPE html>
<html>
  <head>
    <title><%= title %></title>
    <link rel='stylesheet' href='/stylesheets/style.css' />
  </head>
  <body>
    <h1><%= title %></h1>
    <p>Welcome to <%= title %></p>
  </body>
</html>
```
↓
変更後
```
<!DOCTYPE html>
<html>
  <head>
    <title><%= title %></title>
    <link rel='stylesheet' href='/stylesheets/style.css' />
  </head>
  <body>
    <h1><%= title %></h1>
    <p>Welcome to <%= title %></p>
    <input type="text" id="message" size="80" maxlength="100">
    <button>send message</button>
    <div>
        <p>Hi, I am John. I am your tutor today. Talk to you soon!</p>
    </div>
    <script src="https://code.jquery.com/jquery.min.js"></script>
    <script src="/socket.io/socket.io.js"></script>
    <script>
    var socket = io.connect();
    $('button').on('click', function() {
      var text = $('#message').val();
      $('div').append('<p>' + text + '</p>'); 
      socket.emit('request', text);
    });
    socket.on('response', function(param) {
      $('div').append('<p>' + param + '</p>'); 
    });
    </script>
  </body>
</html>
```

#### route/index.jsを修正
変更前
```
var express = require('express');
var router = express.Router();

/* GET home page. */
router.get('/', function(req, res, next) {
  res.render('index', { title: 'Express' });
});

module.exports = router;
```
↓
変更後
```
var express = require('express');
var router = express.Router();

/* GET home page. */
router.get('/', function(req, res, next) {
  res.render('index', { title: 'chat' });
});

module.exports = router;
```

#### bin/wwwを修正
変更前
```
/**
 * Listen on provided port, on all network interfaces.
 */

server.listen(port);
server.on('error', onError);
server.on('listening', onListening);

/**
 * Normalize a port into a number, string, or false.
 */
```
↓
変更後
```
/**
 * Listen on provided port, on all network interfaces.
 */

server.listen(port);
server.on('error', onError);
server.on('listening', onListening);

var socketIO = require('socket.io');
var io = socketIO.listen(server);
io.on('connection', function(socket) {
  socket.on('request', function(param) {
    socket.broadcast.emit('response', param);
  });
});
```

修正を反映するためにWEBサーバーの再起動をする。  
(サーバーサイドのファイルを修正した場合は必ず反映するために再起動を行います。)
```
$ SET DEBUG=learning:* & npm start
```
WEBアプリへアクセスし以下の画面が表示すれば修正完了です。  

 ![img](./img/contents/sc.png "img")
  
### ②リアルタイム通信をしてみる

1.ブラウザを画面に並列に配置する  

 ![img](./img/contents/sc-bo.png "img")

2.片方のテキストに「Hi, John.」と入力し「send message」ボタンを押す
 ![img](./img/contents/sc-in.png "img")

3.両方のブラウザにメッセージが表示される。 
 ![img](./img/contents/sc-me.png "img")

### チャレンジ
*** localhostをコンピュータのIPアドレスに変更しスマホや他のPCからお互いに通信できるのでやって見ましょう。 ***
  