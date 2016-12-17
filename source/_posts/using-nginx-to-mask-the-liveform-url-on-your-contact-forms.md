---
title: Using nginx to mask the liveform url on your contact forms
date: 2016-12-18 00:26:54
tags:
- Nginx
- Mask
---

You can use Nginx to mask the form id of your LiveForm url. Here is a configuration which lets you do that.


```
server {
  server_name awesome.com;
  # ....

  location /form {
    proxy_pass https://liveformhq.com/form/369c5f72-7940-4d20-b3c7-8a963fc49a15;

    proxy_set_header  X-Real-IP        $remote_addr;
    proxy_set_header  X-Forwarded-For  $proxy_add_x_forwarded_for;
    proxy_set_header  Host             $http_host;
    proxy_set_header X_FORWARDED_PROTO $scheme;
  }
}
```

Now you can just use the `/form` endpoint in your html. e.g.

```html
<form method='post' action='/form'>
</form>
```

