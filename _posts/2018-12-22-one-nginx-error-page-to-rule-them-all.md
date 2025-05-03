---
title: One NGINX error page to rule them all
---

When I setup an NGINX server I have to setup custom error pages for every error. I want to show the error message on the page and not have a genenic _"Something is wrong"_ error page. If customers complain about an error page it's nice if they can communicate you the error code or message.

## Before & after

![](/images/posts/nginx-error-page/404-default.png)

![](/images/posts/nginx-error-page/404-styled.png)

## NGINX error page config

First of all you need to create an error_page in your `http`, `server`, or `location` directive ([docs](https://nginx.org/en/docs/http/ngx_http_core_module.html#error_page)), _the `location` block should be part of the `server` directive_:

```nginx
error_page 400 401 402 403 404 405 406 407 408 409 410 411 412 413 414 415 416 417 418 421 422 423 424 425 426 428 429 431 451 497 500 501 502 503 504 505 506 507 508 510 511 /error.html;

location = /error.html {
  ssi on;
  internal;
  auth_basic off;
  root /var/www/default;
}
```

## Error page template

After this you create a `error.html` file in `/var/www/default/` (you can change this directory of course). At [Simple Analytics](https://simpleanalytics.io/?ref=blog.adriaan.io) we deploy our app to our server and for one second NGINX will return a 502 error because the app is reloading. We inform our users with a custom error message and we automatically refresh the page every 2 seconds:

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Your Business</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!--# if expr="$status = 502" -->
      <meta http-equiv="refresh" content="2">
    <!--# endif -->
  </head>
<body>
  <!--# if expr="$status = 502" -->
    <h1>We are updating our website </h1>
    <p>This is only for a few seconds, you will be redirected.</p>
  <!--# else -->
    <h1><!--# echo var="status" default="" --> <!--# echo var="status_text" default="Something goes wrong" --></h1>
  <!--# endif -->
</body>
</html>
```

## NGINX error codes map

To get the variable `status_text` to work you will need this map in your the http directive ([docs](https://nginx.org/en/docs/http/ngx_http_map_module.html)):

```nginx
map $status $status_text {
  400 'Bad Request';
  401 'Unauthorized';
  402 'Payment Required';
  403 'Forbidden';
  404 'Not Found';
  405 'Method Not Allowed';
  406 'Not Acceptable';
  407 'Proxy Authentication Required';
  408 'Request Timeout';
  409 'Conflict';
  410 'Gone';
  411 'Length Required';
  412 'Precondition Failed';
  413 'Payload Too Large';
  414 'URI Too Long';
  415 'Unsupported Media Type';
  416 'Range Not Satisfiable';
  417 'Expectation Failed';
  418 'I\'m a teapot';
  421 'Misdirected Request';
  422 'Unprocessable Entity';
  423 'Locked';
  424 'Failed Dependency';
  425 'Too Early';
  426 'Upgrade Required';
  428 'Precondition Required';
  429 'Too Many Requests';
  431 'Request Header Fields Too Large';
  451 'Unavailable For Legal Reasons';
  500 'Internal Server Error';
  501 'Not Implemented';
  502 'Bad Gateway';
  503 'Service Unavailable';
  504 'Gateway Timeout';
  505 'HTTP Version Not Supported';
  506 'Variant Also Negotiates';
  507 'Insufficient Storage';
  508 'Loop Detected';
  510 'Not Extended';
  511 'Network Authentication Required';
  default 'Something is wrong';
}
```

Reload your NGINX config (no need to restart NGINX):

```bash
sudo nginx -t && sudo nginx -s reload
```

## Test your config

Add this to your server directive:

```
location = /404.html {
  return 404;
}
```

And reload your NGINX again:

```bash
sudo nginx -t && sudo service nginx reload
```

## Useful links

 - [nginx.org/en/docs/http/ngx_http_ssi_module.html](https://nginx.org/en/docs/http/ngx_http_ssi_module.html)
 - [nginx.com/resources/wiki/start/topics/examples/dynamic_ssi/](https://www.nginx.com/resources/wiki/start/topics/examples/dynamic_ssi/)
 - [cambus.net/nginx-and-server-side-includes/](https://www.cambus.net/nginx-and-server-side-includes/)
