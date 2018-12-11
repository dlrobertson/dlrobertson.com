---
layout:   post
type:     blog
title:    Easter Egg
date:     2018-06-06
summary:  Random easter egg I found while frolicking on the web.
keywords: easter egg, unicorn, http, time.com
image:    time-unicorn.png
---

Someone super cool must work at [time.com]. Nerds are awesome!

```
$ nc time.com 80
GET / HTTP/1.1
Host: time.com
Range: bytes=15-878

HTTP/1.1 206 Partial Content
Content-Type: text/html; charset=utf-8
Content-Length: 864
Connection: keep-alive
Date: Fri, 06 Jul 2018 14:17:00 GMT
Server: nginx/1.10.3 (Ubuntu)
X-Powered-By: Element
Cache-Control: max-age=30
ETag: W/"2565b-q2j/xPV/8VrujPqPRZriWXehMJU"
Age: 21
Vary: Accept-Encoding,Accept-Encoding
Content-Range: bytes 15-878/153179
X-Cache: Hit from cloudfront
Via: 1.1 6c3617aad8059817719e72cab06f7158.cloudfront.net (CloudFront)
X-Amz-Cf-Id: AWxLrvL00vf7aJ1H7O6pwZ46QOvbobPf2rdTnfeK1_8tCBxRAlr7iA==


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

[time.com]: https://time.com
