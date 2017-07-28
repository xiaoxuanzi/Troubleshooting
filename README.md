<!-- markdown-toc start - Don't edit this section. Run M-x markdown-toc-generate-toc again -->
**Table of Contents**

- [Troubleshooting](#Troubleshooting)
    - [1 Nginx/OpenResty](#1-Nginx/OpenResty)
        - [1 Nginx内置变量`$upstream_addr` 怎么会包含upstream名字](#1-Nginx内置变量'$upstream_addr'怎么会包含upstream名字)
    - [2 Go](#2-Go)
    - [3 Python/Flask/Django](#3-Python/Flask/Django)
    - [4 C/C++](#4-C/C++)
    - [5 Java](#4-Java)

# Troubleshooting

## 1 Nginx/OpenResty
### 1 Nginx内置变量`$upstream_addr`怎么会包含upstream名字
$upstream_addr 值代表upstream的地址，官方文档是这样解释的：
<pre><code>
keeps the IP address and port, or the path to the UNIX-domain socket of the upstream server (1.11.4). If several servers were contacted during proxying, their addresses are separated by commas, e.g. “192.168.1.1:12345, 192.168.1.2:12345, unix:/tmp/sock”.(http://nginx.org/en/docs/http/ngx_http_upstream_module.html)
</pre></code>
在线上会发现使用 log_format '$upstream_addr'; 输出会包含 upstream的名字。原因是：
<pre><code>
If an error occurs during communication with a server, the request will be passed to the next server, and so on until all of the functioning servers will be tried. If a successful response could not be obtained from any of the servers, the client will receive the result of the communication with the last server.(http://mailman.nginx.org/pipermail/nginx/2016-February/049800.html)
</pre></code>
