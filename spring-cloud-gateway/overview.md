# 概要

该项目（Spring Cloud Gateway ）旨在建立一种用于Spring WebFlux的API网关库，它是一种简单且高效的API路由及转发中心。

### 特性 <a id="features"></a>

Spring Cloud Gateway 的特性:

* 基于Framework 5，Reactor技术及Spring Boot 2.0构建
* 支持匹配路由以及任意的Request请求属性
* 具有断言以及过滤器的能力
* 集成了断路器功能
* 集成Spring Cloud 注册发现功能
* 可以快速自定义断言及过滤器
* 支持简单的限流功能
* 具有路径重写能力

### 开始学习 <a id="getting-started"></a>

```text
@SpringBootApplication
public class DemogatewayApplication {
	@Bean
	public RouteLocator customRouteLocator(RouteLocatorBuilder builder) {
		return builder.routes()
			.route("path_route", r -> r.path("/get")
				.uri("http://httpbin.org"))
			.route("host_route", r -> r.host("*.myhost.org")
				.uri("http://httpbin.org"))
			.route("rewrite_route", r -> r.host("*.rewrite.org")
				.filters(f -> f.rewritePath("/foo/(?<segment>.*)", "/${segment}"))
				.uri("http://httpbin.org"))
			.route("hystrix_route", r -> r.host("*.hystrix.org")
				.filters(f -> f.hystrix(c -> c.setName("slowcmd")))
				.uri("http://httpbin.org"))
			.route("hystrix_fallback_route", r -> r.host("*.hystrixfallback.org")
				.filters(f -> f.hystrix(c -> c.setName("slowcmd").setFallbackUri("forward:/hystrixfallback")))
				.uri("http://httpbin.org"))
			.route("limit_route", r -> r
				.host("*.limited.org").and().path("/anything/**")
				.filters(f -> f.requestRateLimiter(c -> c.setRateLimiter(redisRateLimiter())))
				.uri("http://httpbin.org"))
			.build();
	}
}
```

