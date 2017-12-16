---
title: 通过CROS协议解决跨域问题
categories: java
date: 2017-07-23 21:21:48
tags:
---

在前后端交互的开发中可能会遇到跨域的问题，如果只是简单的GET请求的话可以利用Jsonp来解决。

对于非GET请求的话就可以采用CORS协议来解决了。CORS是一个W3C标准，全称是"跨域资源共享"（Cross-origin resource sharing）。
它允许浏览器向跨源服务器，发出XMLHttpRequest请求，从而解决了只能同源使用的限制，具体详解参考[跨域资源共享 CORS 详解](http://www.ruanyifeng.com/blog/2016/04/cors.html)。

如果你需要信息的绝对安全，不要依赖CORS当中的权限制度，应当使用更多其它的措施来保障，比如OAuth2。

对于java服务器的话，常用的解决方案就是自定义个Filter来添加相应头。

```
@Component
public class CorsFilter implements Filter {
	private static final Logger logger = LoggerFactory.getLogger(CorsFilter.class);

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        HttpServletResponse response = (HttpServletResponse) servletResponse;
        String origin = (String) servletRequest.getRemoteHost()+":"+servletRequest.getRemotePort();
        logger.info("orgin: {} request cors resource.", origin);
        response.setHeader("Access-Control-Allow-Origin", "*");
        response.setHeader("Access-Control-Allow-Methods", "POST, GET, OPTIONS, DELETE");
        response.setHeader("Access-Control-Max-Age", "3600");
        response.setHeader("Access-Control-Allow-Headers", "x-requested-with,Authorization");
        response.setHeader("Access-Control-Allow-Credentials","true");
        filterChain.doFilter(servletRequest, servletResponse);
    }

    @Override
    public void destroy() {

    }
}

```
OPTIONS是预请求必须允许。
Authorization是做了oauth2登录响应所必须的。
预请求在实际请求之前发出的请求，为了保证实际请求能够完成的权限请求，通过预请求的响应将能够确定实际请求是否的完成。

```
// 预请求
OPTIONS /cors HTTP/1.1
Origin: http://api.alice.com
// 实际请求类型
Access-Control-Request-Method: POST
Access-Control-Request-Headers: X-Custom-Header
Host: api.bob.com
Accept-Language: en-US
Connection: keep-alive
User-Agent: Mozilla/5.0...


// 预请求响应
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET, POST, PUT
Access-Control-Allow-Headers: X-Custom-Header
Access-Control-Allow-Credentials（可选）- 表示是否允许cookies
Access-Control-Max-Age（可选） – 以秒为单位的缓存时间，允许时应当尽可能缓存。
Content-Type: text/html; charset=utf-8
```


```
<filter> 
	<filter-name>cors</filter-name> 
	<filter-class>CorsFilter</filter-class> 
</filter>

<filter-mapping>
	<filter-name>cors</filter-name>
	<url-pattern>/api/*</url-pattern>
</filter-mapping>
```

在Spring中提供了更为简单便捷的方法。
1. 使用@CrossOrigin注解来设置跨域访问所允许的域名
2. 继承WebMvcConfigurerAdapter设置跨域相关配置
```
@Component
public class CorsConfigurerAdapter extends WebMvcConfigurerAdapter {

	@Override public void addCorsMappings(CorsRegistry registry) {
	 registry.addMapping("/api/*").allowedOrigins("*"); 
	 } 
 }
```

详细参考[Spring官方文档](http://spring.io/guides/gs/rest-service-cors)