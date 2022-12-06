---
layout:     post
title:      "HTTP Status Code들"
subtitle:   "Status Code들에 대한 정리"
author:     "H37-J"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - web
---

### 200(OK)

요청을 성공 하였음을 의미

### 400(Bad Request)

요청을 할 때 요청 메시지가 잘못 되었거나 라이퉁이 변조되어 서버가 요청을 이해할 수가 없음

### 401(Unauthorized)

요청을 하는 사람이 인증을 받지 못해서 해당 권한이 없음

### 403(Forbidden)

인증을 받았지만 인가를 받지 못하여 권한이 없음

### 404(Not Found)

요청 받은 페이지나 리소스가 없음

### 405(Method Not Allowed)

해당 요청 메소드가 있긴 하지만 사용을 허가받지 못할 때의 에러

### 500(Internal Server Error)

서버가 해당 상황을 어떻게 처리해야 할지 모를 때 발생 하는 에러

### 502(Bad Gateway)

게트웨이에서 잘못된 응답을 수신하였음을 의미

### 503(Service Unavaliable)

서버가 해당 요청을 처리할 준비가 되지 않았음을 의미
