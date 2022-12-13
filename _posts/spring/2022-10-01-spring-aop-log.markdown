---
layout:     post
title:      "AOP를 이용하여 접속 사용자 로그 남기기"
subtitle:   "클라이언트 로그 추적"
author:     "H37-J"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - spring
---

### 코드요약

포인트컷으로 내가 사용하는 모든 컨트롤러 호출시 해당 어드바이스가 실행 되도록 하였다.
arounLog 메소드에 해당 비즈니스 로직을 실행하면 ip, 클라이언트 브라우저, 호출 httpmethod등을 알 수 있다.
해당 결과 값은 log.json에 따로 추가 되도록 만들었다.

```java
@Aspect
@Slf4j
@Component
public class LogginAspect {

    private static final String UNKNOWN = "unknown";
    

    @Pointcut("execution (* com.hjk.controller.*Controller.*(..))")
    public void log() {
    }
 
    @Around("log()")
    public Object aroundLog(ProceedingJoinPoint point) throws Throwable {

        ObjectMapper mapper = new ObjectMapper();
        ServletRequestAttributes attributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
        HttpServletRequest request = Objects.requireNonNull(attributes).getRequest();
 
        long startTime = System.currentTimeMillis(); //실행시간 측정 시작
        Object result = point.proceed(); //접속
        String userAgent = request.getHeader("User-Agent"); //브라우저

        final Log l = Log.builder().threadId(Long.toString(Thread.currentThread().getId()))
        .threadName(Thread.currentThread().getName())
        .ip(getIp(request)).url(request.getRequestURL().toString())
        .httpMethod(request.getMethod())
        .timeCost((System.currentTimeMillis() - startTime) / 1000.0)
        .createdAt(DateUtils.LocalDateFormat(LocalDateTime.now()))
        .userAgent(userAgent)
        .build();

        log.info("요청 로그 : {}", l.toString());

        Map<Integer,Object> add_map = new HashMap<>();
        Map<Integer,Object> current_map = mapper.readValue(new File("log.json"),Map.class);
 
        int num= 1;
 
        if(!current_map.isEmpty()) {
            for (Map.Entry<Integer, Object> entry : current_map.entrySet()) {
                add_map.put(num, entry.getValue());
                num++;
            }
        }
 
        add_map.put(num,l);
        mapper.writerWithDefaultPrettyPrinter().writeValue(new File("log.json"), add_map);
 
        return result;
    }



    public static String getIp(HttpServletRequest request) {
        String ip = request.getHeader("x-forwarded-for");
        if (ip == null || ip.length() == 0 || UNKNOWN.equalsIgnoreCase(ip)) {
            ip = request.getHeader("Proxy-Client-IP");
        }
        if (ip == null || ip.length() == 0 || UNKNOWN.equalsIgnoreCase(ip)) {
            ip = request.getHeader("WL-Proxy-Client-IP");
        }
        if (ip == null || ip.length() == 0 || UNKNOWN.equalsIgnoreCase(ip)) {
            ip = request.getRemoteAddr();
        }
        String comma = ",";
        String localhost = "127.0.0.1";
        if (ip.contains(comma)) {
            ip = ip.split(",")[0];
        }
        if (localhost.equals(ip)) {
            try {
                ip = InetAddress.getLocalHost().getHostAddress();
            } catch (UnknownHostException e) {
                log.error(e.getMessage(), e);
            }
        }
        return ip;
    }


    @Builder
    @NoArgsConstructor
    @AllArgsConstructor
    @Getter
    static class Log {
 
        private String threadId;
 
        private String threadName;

        protected String createdAt;
 
        private String ip;
 
        private String url;
 
        private String httpMethod;
 
        private Double timeCost;

        private String userAgent;
 
        @Override
        public String toString() {
            return "스레드: " + this.threadId + "," + "스레드네임: " + this.threadName + "," + "아이피: " + this.ip + "," + "URI: "
                    + this.url + "," + "메소드: " + this.httpMethod + ", 클래스 메소드: " + ", 파라미터: "
                    +", 실행시간: " + this.timeCost + ", 유저정보: " + this.userAgent + ", 요청시간: " + this.createdAt;
        }
    }

}
```

컨트롤러가 호출 되면 루트 경로에 log.json파일에 다음과 같이 결과 값이 추가된다.

```json 

{
  "1" : {
    "threadId" : "251",
    "threadName" : "http-nio-8080-exec-3",
    "createdAt" : "2022-12-10 22:05:33",
    "ip" : "0:0:0:0:0:0:0:1",
    "url" : "http://localhost:8080/api/shop/product/get/bestProduct",
    "httpMethod" : "GET",
    "timeCost" : 0.005,
    "userAgent" : "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/107.0.0.0 Safari/537.36"
  },
  "2" : {
    "threadId" : "296",
    "threadName" : "http-nio-8080-exec-10",
    "createdAt" : "2022-12-10 22:10:21",
    "ip" : "0:0:0:0:0:0:0:1",
    "url" : "http://localhost:8080/api/shop/product/category/list",
    "httpMethod" : "GET",
    "timeCost" : 0.008,
    "userAgent" : "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/107.0.0.0 Safari/537.36"
  },
}
```