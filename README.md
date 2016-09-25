# guide to integrating with wechat (weixin) platform

[Wechat](http://www.wechat.com/en/) is one of the largest messaging app by user count but its API and [docs](http://admin.wechat.com/wiki/index.php?title=Main_Page) are not very developer-friendly. It is especially hard for non-Chinese speaking developers. This guide is my attempt to save others some pain and effort. Some solutions are opinionated and you are welcome to submit your suggestions via [issues](https://github.com/falcondai/wechat-nodejs-guide/issues).

## public accounts

To register a (personal) public account, you will need a **Chinese national ID**. And there is a quota (currently 5) on how many accounts each ID can create. If you just want to play with the public account API and test your integration, you should start with [the official sandboxed test accounts](http://mp.weixin.qq.com/debug/cgi-bin/sandbox?t=sandbox/login) which only requires you to have a wechat account to create.

### register a test account
- you need to click the green button and scan the shown QR code. After this step, you should see the test account management panel.
- you can subscribe to this test account by scanning the QR code shown under the `测试号二维码` panel. The test account will have a name like `sandbox account of X`.

### setup webhook
- you will need a **publicly accessible IP** to continue. My suggestion is to use [ngrok](ngrok.com) to tunnel your local development server to a public URL.
  - suppose that your local dev server is listening on port `3000`, run `$ ngrok http 3000` and you will be automatically assigned a public forwarding domain that looks like `your.ngrok.io`.
  - enter `your.ngrok.io/wechat` in the `接口配置信息` panel. As you might have guessed, we will be handling requests from wechat servers at the `/wechat` endpoint.
  - enter some random string - let's say `token` - as token in the `接口配置信息` panel. This string will serve as a secret for you to verify the identity of wechat servers.
  - (we will come back to click the green button to complete the webhook setup.)
- now, we will start integrating with wechat. If you use [express](http://expressjs.com), instead of deciphering the [docs](http://mp.weixin.qq.com/wiki/17/2d4265491f12608cd170a95559800f2d.html), you might want to use this node module [wechat](https://www.npmjs.com/package/wechat) to do most of the work for you `$ npm install wechat --save`.
- as a minimal example, create `app.js`:

```javascript
var express = require('express');
var wechat = require('wechat');

var app = express();

app.use('/wechat', wechat('token', function (req, res, next) {
  // message is located in req.weixin
  var message = req.weixin;
  console.log(message);
}));

app.listen(3000);
```
- run your app with `$ node app.js` or even better `$ supervisor app.js`
- go back to the management panel and click the green button in the `接口配置信息` panel to complete the webhook setup. It should show a green popup message briefly and the clicked button will disappear.
- go to your test account on you wechat app and try sending a few messages. Watch the received message JSON objects show up in your terminal.
