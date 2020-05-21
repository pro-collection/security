## spring-security 学习


### 项目搭建
直接看pom 文件就可以了

#### hello-word
- demo/src/main/java/com/imooc/DemoApplication.java
- 解决问题： 数据库链接
- 解决问题： 暂时关闭session spring.session.store-type = none
- 直接访问： 127.0.0.1:8080
- 会默认要登录验证， 关闭就行： security.basic.enabled = false

#### 打包发布
在福羡慕， 执行maven-build 命令就可以了。
打包之后，每个文件夹都多了一个target的打包结果（不符合预期）
要在`yanle-security-demo`中的pom文件加入这样的配置
```xml
	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<executions>
					<execution>
						<goals>
							<goal>repackage</goal>
						</goals>
					</execution>
				</executions>
			</plugin>
		</plugins>
		<finalName>demo</finalName>
	</build>
```

- idea 中的打包： `yanle-security` -> maven -> Lifecycle -> package  

### api
- 建立一个测试类： demo/src/test/java/com/imooc/web/controller/UserControllerTest.java
- 需要伪造一个 WebApplicationContext环境， 这样启动快
- 做一个查询方法测试： demo/src/test/java/com/imooc/web/controller/UserControllerTest.java - whenQuerySuccess 
- 添加controller: demo/src/main/java/com/imooc/web/controller/UserController.java
- 涉及到的结构体，定义在 demo/src/main/java/com/imooc/dto/User.java
- 涉及到参数的接受类： demo/src/main/java/com/imooc/dto/UserQueryCondition.java
- 错误处理机制
    - demo/src/main/java/com/imooc/exception/UserNotExistException.java
    - demo/src/main/java/com/imooc/web/controller/ControllerExceptionHandler.java
- 切片拦截
    - 过滤器拦截： demo/src/main/java/com/imooc/web/filter/TimeFilter.java
    - 如果添加三方的过滤器： demo/src/main/java/com/imooc/web/config/WebConfig.java
    - 拦截器： demo/src/main/java/com/imooc/web/interceptor/TimeInterceptor.java
        - 拦截器是需要添加到配置的， 否则无法使用
        - 项目与过滤去， 好处是， 可以拿到方法的调用
        - 缺点是无法拿到参数
    - 切片AOP： 
        - demo/src/main/java/com/imooc/web/aspect/TimeAspect.java
- 文件服务
    - 测试上传文件： demo/src/test/java/com/imooc/web/controller/UserControllerTest.java whenUploadSuccess
    - 添加DTO： demo/src/main/java/com/imooc/dto/FileInfo.java
    - 上传下载接口： demo/src/main/java/com/imooc/web/controller/FileController.java
- 多线程
    - 异步请求： demo/src/main/java/com/imooc/web/async/AsyncController.java
        - Callable 方式处理异步请求
        - DeferredResult 方式处理异步请求
    - 模拟消息队列： demo/src/main/java/com/imooc/web/async/MockQueue.java
    - 处理消息通信： demo/src/main/java/com/imooc/web/async/DeferredResultHolder.java
    - 监听器： demo/src/main/java/com/imooc/web/async/QueueListener.java
    - 修改配置： demo/src/main/java/com/imooc/web/config/WebConfig.java
- Swagger文档工具
    - 参考文章：[SwaggerUI](https://github.com/demo-collection/web-document/blob/master/document-demo/docs/02%E3%80%81SwaggerUI.md)
- WireMock
    - 链接本地文件： dmeo/src/main/java/com/imooc/wiremock/MockServer.java
        这种方式其实并不好，前端有更多更好的办法做mock服务
        如果实在是想用 WireMock 要去官网看看

