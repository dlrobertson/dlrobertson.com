---
layout:   post
type:     blog
title:    Easter Egg
date:     2018-06-06
summary:  Random easter egg I found while frolicking on the web.
---

Someone super cool must work at [time.com]. Nerds are awesome!

```
$ nc time.com 80
GET / HTTP/1.1
Host: time.com
Range: bytes=0-878

HTTP/1.1 206 Partial Content
Content-Type: text/html; charset=utf-8
Content-Length: 879
Connection: keep-alive
Date: Fri, 06 Jul 2018 13:00:59 GMT
Server: nginx/1.10.3 (Ubuntu)
X-Powered-By: Element
Cache-Control: max-age=30
ETag: W/"256dd-O99IEr8kKSi6Rc57DC/c5FFiOl0"
Vary: Accept-Encoding,Accept-Encoding
Content-Range: bytes 0-878/153309
X-Cache: Miss from cloudfront
Via: 1.1 5ae8b0e76af80aa471660f48d35acb41.cloudfront.net (CloudFront)
X-Amz-Cf-Id: lXOPkSkfsq62eGblqaNCAqBjxjBvaC7HLFeIUIL8wpOf7m9sB3MCRg==

<!doctype html>
<!--
        /((((((\\\\
=======((((((((((\\\\\
     ((           \\\\\\\
     ( (*    _/      \\\\\\\
       \    /  \      \\\\\\________________
        |  |   |       </                  ((\\\\
        o_|   /        /                      \ \\\\    \\\\\\\
             |  ._    (                        \ \\\\\\\\\\\\\\\\
             | /                       /       /    \\\\\\\     \\
     .______/\/     /                 /       /         \\\
    / __.____/    _/         ________(       /\
   / / / ________/`---------'         \     /  \_
  / /  \ \                             \   \ \_  \
 ( <    \ \                             >  /    \ \
  \/     \\_                           / /       > )
          \_|                         / /       / /
                                    _//       _//
                                   /_|       /_|
-->
```

[time.com]: time.com
