# guide to integrating with wechat (weixin) platform

[Wechat](http://www.wechat.com/en/) is one of the largest messaging app by user count but its API and [docs](https://mp.weixin.qq.com/) are not very developer-friendly. It is especially hard for non-Chinese speaking developers. This guide is my attempt to save others some pain and effort. Some solutions are opinionated and you are welcome to submit your suggestions via [issues](https://github.com/falcondai/wechat-nodejs-guide/issues).

## public accounts

To register a (personal) public account, you will need a Chinese national ID. And there is a quota (currently 5) on how many accounts each ID can create. If you just want to play with the public account API and test your integration, you should start with [the official sandboxed environment](http://mp.weixin.qq.com/debug/cgi-bin/sandbox?t=sandbox/login).

- you need to click the green button and scan the shown QR code. After this step, you should see the testing account management panel.
- you can subscribe to this testing account by scanning the QR code shown under the `测试号二维码` panel.
- you will need a publicly accessible IP to continue. My suggestion is to use [ngrok](ngrok.com) to tunnel your local development server to a public URL.
  - suppose that your local dev server is listening on port 3000, run `$ ngrok http 3000` and you will be automatically assigned a public forwarding domain that looks like `123abc.ngrok.io`
  - enter that address in the `接口配置信息` panel.
