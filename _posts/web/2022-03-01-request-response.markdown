---
layout:     post
title:      "웹에서 요청과 응답이 이루어지는 과정"
subtitle:   "요청과 응답에 대한정리"
author:     "H37-J"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - web
---

### 요청 및 응답에 대한 과정

클라이언트는 웹을 사용하면서 요청 메세지를 서버에 보내게 된다.
그럼 서버는 그 요청 메시지에 따라서 서버 내에서 요청을 처리하고 그에 대한 결과를 다시 클라이언트에게 돌려준다.

### 요청 메시지의 구조

``` terminal
GET https://test.com //GET은 요청 메서드, 그리고 URL 주소이다

//다음으로 헤더가 온다
Accept: text/html, application/xml, image/webp, image.apng
Accept-Encoding: gzip, deflate
Host: www.test.com
Content-Type: application/json
User-Agent: Mozilla

//그리고 본문이 온다
name=test&age=22
```

### 응답 메시지 구조

```terminal
// 처음으로 헤더가 온다
HTTP/1.1 200 OK
text/html; charset=UTF-8
Date: Tue, 06 Dec 2022 21:10:04 GMT
Vary: Accept-Encoding
Content-Type: text/html; charset=UTF-8
Content-Lenght: 648

// 그리고 본문이 온다
<!doctype html>
<html>
<head>
    <title>Test Site</title>
</body>
</html>
```

### HTTPS란?

HTTP에는 다음과 같은 문제가 있었다
* 메시지 본문이 도청 될 수 있슴
* 스푸핑의 위험이 있다
* 무결성을 증명할 수 없으며 메시지가 변조 되었을수 있다

HTTPS는 SSL과 통신한 다음 SSL이 TCP와 통신하도록 허용 하는것이다. HTTPS를 이용하면 도청 방지, 스푸핑 방지, 변조방지를 할 수 있다

### HTTP 2.0

HTTP 1.x에는 다음과 같은 문제가 있었다.  
1. 요청 및 응답 헤더를 압축하지 않아 불필요한 네트워크 트래픽이 많았다.  
2. 리소스 우선 순위 지정이 되지 않아 기본 TCP연결이 충분히 활용되지 않았다.

HTTP2.0 이런 문제들을 해결한다. 또한, 헤더 프레임과 데이터 프레임을 구분한다.
