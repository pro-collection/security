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




