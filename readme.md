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


### 基于Spring Security 开发表单验证
核心功能：               
认证（登录）、授权、攻击保护

- 基本原理                                      
如果什么都不做的时候 `security.basic.enabled = true` 是这样的， 如果希望关闭就直接 设置为false , 如果是默认的场景， 访问接口的时候， 会让填写用户名和密码。
用户名是默认的， 用户名是默认的 user, 密码在启动的时候， 会给出来。
    
    - 基本配置： browser/src/main/java/com/imooc/security/browser/BrowserSecurityConfig.java
        - 主要配置http请求相关： browser/src/main/java/com/imooc/security/browser/BrowserSecurityConfig.java configure

- 自定义用户认证
    - 用户信息获取： demo/src/main/java/com/imooc/security/MyUserDetailsService.java                                     
    - 用户的校验：                                                    
        - 密码是否正确的部分是由springSecurity 验证好了的。                                                   
        - 校验逻辑：  SocialUser 其中就是 3，4，5，6 四个参数                                   
        - 加密： 加密哟啊使用到 PasswordEncoder                                   
            需要添加到配置里面去： browser/src/main/java/com/imooc/security/browser/BrowserSecurityConfig.java `public PasswordEncoder passwordEncoder()`
            
- 个性化用户认证
    - 自定义登录页面： browser/src/main/java/com/imooc/security/browser/BrowserSecurityConfig.java    
        - 自定义controller, 如果没有登录， 就直接返回错误码， 让前端自己跳转到对应的登录页面。
    - 自定义验证的controller： browser/src/main/java/com/imooc/security/browser/BrowserSecurityController.java                                  
        如果用户没有登录， 直接返还状态码， 让前端自己做跳转判定
    - 添加一些外部调用的可以配置的参数： core/src/main/java/com/imooc/security/core/properties/SecurityProperties.java                   
                                     core/src/main/java/com/imooc/security/core/properties/BrowserProperties.java                                                           
    - 添加配置的扫描： core/src/main/java/com/imooc/security/core/SecurityCoreConfig.java
    
    
    
    
    