# nginx一个端口部署多个单页应用

## 应用场景

有时候业务需求会需要一些简单的单页应用作为展示，但单页应用的累积很容易混淆端口的分配，所以最好是能在一个端口上分发不同的页面应用

## 方法

### 使用子域名区分

这种方法很简单，但限制很大，要么更改本地的hosts文件，要么需要购买域名

```nginx
server {
    listen        80;
    server_name   aa.test.com;  # 子域名aa访问时
    location / {
        root         html/testaa;
        try_files    $uri /index.html index.htm;
    }
}

server {
    listen        80;
    server_name   bb.test.com;  # 子域名bb访问时
    location / {
        root         html/testbb;
        try_files    $uri /index.html index.htm;
    }
}
```

### 采用路径的文件夹区分

比如http://aa.com/a/xx和http://aa.com/b/xx采用a和b区分不同的项目

```nginx
server {
    listen 80;
    root html/test;
    location ^~ /a/ {
        try_files $uri /a/index.html;
    }
    location ^~ /b/ {
        try_files $uri /b/index.html;
    }
}
```

