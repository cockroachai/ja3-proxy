# ja3-proxy
使用ja3越过cf验证,5秒盾使用yes通过验证

## 交流群
https://t.me/xyhelper

## 需准备的资源
一个可访问目标网站(例如 https://chat.openai.com )的代理服务器,注意不要使用动态IP代理,可能因IP跳动导致过盾失效,如果要使用平台过盾,需满足以下条件:
```
代理地址，支持以下格式：

    有密码http/https代理：http://user:pass@45.91.239.47:62930 

    没有密码http/https代理：http://45.91.239.47:62930

    没有密码的socks5代理： socks5://5.252.190.52:64585 

    ！不支持带密码的socks5代理！

注意：如果需要权限，请将43.154.193.54加入白名单

注意：CF盾对代理要求较高，请使用国际代理，如果报ERROR_CAPTCHA_UNSOLVABLE错误，请更换代理再试一下，也可以联系我们测试是否能过（绝大部份情况都能过）

注意：不要使用本地代理（127.0.0.1、localhost、192.168.x.x、172.0.x.x），本地代理只有你自己电脑才能访问，服务器访问不了！
```

一个`yescaptcha`的`clientKey`,新用户请使用我的推荐链接 [https://yescaptcha.com/i/RLazNv](https://yescaptcha.com/i/RLazNv) 注册,可以直接获得`VIP5`等级,注意成功后，联系`yescaptcha`客服可获取赠送的1500积分。

## 使用方法

创建 `docker-compose.yml`
```yml
version: '3'
services:
  ja3-proxy:
    image: xyhelper/ja3-proxy
    ports:
      - "16524:16524"
      - "9988:9988"
    environment:
      WEBSITE_URL: "https://chat.openai.com/" # 要过盾的目标网站
      PROXY: http://username:password@IP:端口  # 代理服务器信息
      CLIENTKEY: "4733exxxxxx" # yescaptcha 的 clientKey
      LOCALPROXYUSER: "自行设置一个用户名" # ja3代理服务用户名
      LOCALPROXYPASS: "自行设置一个密码" # ja3代理服务密码
```
如不需过盾，可不配置 PROXY和CLIENTKEY


启动
```bash
docker compose up -d
```
查看日志

```bash
docker compose logs -f 
```

当日志中显示以下内容时表示启动完成 `NeedBypass` 表示是否需要过盾 如果 `ServerStatus` 为 0,说明尝试过盾失败，请根据日志进一步排查。
```bash
2024-03-23 04:52:19.419 [INFO] {626c0075af58bf178c52cc75cc68392d} NeedBypass: true ServerStatus: 200
2024-03-23 04:52:19.419 [INFO] {626c0075af58bf178c52cc75cc68392d} start http server
2024/03/23 04:52:19 [::]:9988
2024-03-23 04:52:19.421 [INFO] pid[2482139]: http server started listening on [:16524]
2024-03-23 04:52:19.421 [INFO] {f75e39f16158bf17ad1b3307f36d6d0d} openapi specification is disabled

  ADDRESS | METHOD | ROUTE |  HANDLER  | MIDDLEWARE  
----------|--------|-------|-----------|-------------
  :16524  | ALL    | /ping | main.ping |             
----------|--------|-------|-----------|-------------
```
## 测试服务

```bash
curl --insecure --proxy http://你的用户名:你的密码@127.0.0.1:9988 'https://chat.openai.com/'
```

## 赞助商信息
![](20240206-180420.jpg)

[https://www.capsolver.com/](https://www.capsolver.com/?utm_source=github&utm_medium=banner_github&utm_campaign=ja3-proxy)