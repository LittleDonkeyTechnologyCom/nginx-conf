# nginx 通用配置模板

整理 [nginx][nginx] 配置模板，方便新项目复用，在此感谢 [旅销宝][旅销宝]，让我有机会使用 [nginx][nginx] 配置 https 服务，并且推广的能力。

> 本项目仅在 windows 10 上测试成功，虽然我有 linux 服务器，可是我就是不尝试，你能拿我咋地 (#^.^#)。

## 目录结构

```
nginx-conf/
├── url-filters/                 # 过滤器，主要用于拦截异常请求
├── sites-options/               # 公共可选配置
├── sites-enabled/               # 目前启动的站点配置   
├── sites-available/             # 可用站点配置 ( 含废弃，历史，存档等配置 )
|   ├── http-server-tpl.conf
|   └── https-server-tpl.conf     
└── sites-certs/                 # https 证书存放目录
    └── dhparam.pem
```

## 配置 Nginx

> path/to 为 nginx 的安装目录

**1. 在 `path/to/conf/nginx.conf` 文件中添加以下代码**

```bash
# 注意 http 文件中就存在的，是在 http 的内容添加内容
http {
	...

	include ../sites-enabled/*.conf;
	
	...
}
```

**2. 复制项目下的所有文件夹到 [nginx][nginx] 安装目录下**
.
**3. 配置新站点**

 1. 从 `site-available` 复制模板文件到 `site-enabled` 模板
 2. 根据实际项目需要修改里面的 `server_name`
 3. 如果是 https 还需要将证书的路径添加正确

**4. 检查 [nginx][nginx] 是否配置成功**

```bash
# 注意这个只检查语法，但是会忽略使用通配符载入的文件
# 比如: include cc/*.conf, 如果 cc 文件夹不存在也不会报错
# 但是如果是引入单个文件，文件不存在，就会报错
nginx -t
```

**5. 重新加载配置文件**

```bash
nginx -s reload
```

## Nginx 常用命令

```bash
# 检查 nginx 配置是否正确
nginx -t

# 停止服务
nginx -s stop

# 退出服务
nginx -s quit

# 重启服务
nginx -s reload
```

## 本机使用任意域名访问 nginx 服务

> windows/mac 修改本机 hosts 文件，推荐使用 [switchhosts][switchhosts] 软件管理本机 host

```bash
# 可以将 example 改成任意域名
127.0.0.1 example.com
```

在浏览器中使用 `example.com` 就可以访问到 nginx 中配置的项目了

## 注意事项

windows 用户使用命令方式需加上参数 -p 指定 ngixn 路径，-p 指定 nginx 的安装目录，加引号是因为文件名称中间有空格

```bash
# 检查 nginx 配置是否正确
nginx -p "C:\Program Files (x86)\nginx-1.13.8" -t

# 重启服务
nginx -p "C:\Program Files (x86)\nginx-1.13.8" -s reload
```

项目仅参考了 [旅销宝][旅销宝] 的配置，更多的是基于我自身在使用过程中的理解，so，可能会出现很大部分误差，推荐本地开发使用，生产使用请慎重。

在我的 windows 10 上 [nginx][nginx] 不支持 deferred 和 reusepor，linux 阵营的可以去了解下这两个，看介绍貌似挺牛 X 的，值得去尝试。

编写配置时用的 [nginx][nginx] 为 1.13.8 版本，低于或高于这个版本的请自测，不行请自行处理，顺便提供下解决方案，不谢 (#^.^#)！


## Thanks

以下排名不分先后

- [nginx][nginx]
- [旅销宝][旅销宝]
- [nginx-conf][nginx-conf]
- And more open source projects.

## LICENSE

MIT

[旅销宝]: https://www.lxiaobao.com/
[nginx]: http://nginx.org/
[nginx-conf]: https://github.com/carlbennett/nginx-conf
[switchhosts]: https://github.com/oldj/SwitchHosts
