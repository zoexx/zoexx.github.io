title: 使用mailchimp admin权限
date: 2019-09-02 22:09:03
tags: mailchimp
---

一个简单的nginx转发配置

```
server {
    listen 8888;

    location /maillist/subscribe {
        if ($request_method = GET) {
            return 403;
        }

        add_header Access-Control-Allow-Origin * always;
        add_header Access-Control-Allow-Methods PUT,POST,OPTIONS;
        add_header Access-Control-Allow-Headers Content-Type;

        if ($request_method = OPTIONS) {
            return 204;
        }

        proxy_set_header Authorization 'apikey your-api-key';
        rewrite /maillist/subscribe/(.*)$ /3.0/lists/{list-id}/members/$1 break;
        proxy_pass https://us20.api.mailchimp.com;
    }
}
```