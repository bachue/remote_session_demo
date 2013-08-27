## Demo Usage

* set hosts: `127.0.0.1 dev.session.com dev.app1.com dev.app2.com`
* `bundle install`
* `foreman start`
* set session data in app1: `open http://dev.app1.com:4567?name=hooopo`
* read session data from remote in app2: `open http://dev.app2.com:4568`

## Quick View

#### App1
![app1](http://i.imgur.com/4i5gLyU.jpg)

#### App2
![app2](http://i.imgur.com/D8w52Kp.jpg)

#### 技术分析

1. 页面1通过Ajax向Session服务器发送`generate_session_id`请求并且设置callback
2. Session服务器向页面1回复Etag的值，由于这是第一次请求，Session服务器将随机创建一个Etag并将其返回
3. 页面1将传回的Session id设置在cookie中，刷新当前界面。服务器将Session id和参数发送到Session服务器中，Session服务器保存这一值

4. 页面2通过Ajax向Session服务器发送`generate_session_id`请求并且设置callback
5. 由于已经被设置过Etag，Session服务器将Etag返回，作为Session id
6. 页面2将传回的Session id设置在cookie中，reload当前节目。由于页面2没有参数，因此服务器将根据Session id从Session服务器中取回结果。Remote Session的过程完成。
