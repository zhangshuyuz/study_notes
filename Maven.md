# Maven
## 1. Maven介绍
1. Maven介绍
    > 1. 为何使用Maven和Maven有什么用
    > 
    >    使用Maven管理项目，更方便。
    > 
    >    第三方jar包管理
    >    
    >    jar包依赖
    >    
    >    jar包冲突
    >    
    >    获取第三方jar包
    >    
    >    项目拆分为多个模块
    >    
    > 2. Maven是什么
    > 
    >    Maven是帮助程序员进行项目构建和依赖管理的工具。
    >    
### 1. 构建
1. 构建
    > 1. 构建的概念
    > 
    >    构建不是创建
    >    
    > 2. 构建web项目流程
    > 
    >    清理，编译，测试，报告，打包，部署
    >    
    > 3. Maven能够自动构建web项目
    > 
    > 4. 工作空间的项目名，专业术语就叫项目名。部署的项目所在的文件夹名称，专业术语叫做上下文地址，上下文地址用于URL的搜索，可以随便起名。
    > 
## 2. Maven核心概念
### 1. POM
1. POM
    > 1. 介绍
    > 
    >    POM，Project Object Model，项目对象模型
    >    
    > 2. 其他参见文档
    > 
### 2. 约定的目录结构
2. 约定的目录结构
   
    > 1. 参见文档
### 3. 坐标
3. 坐标
   
    > 1. 参见文档
### 4. 依赖管理
4. 依赖管理
    > 1. 解决jar包冲突的几种方式
    > 
    >     1. 最短路径原则：根据最短路径原则，设计jar包
    >     
    >     2. 依赖传递：根据依赖传递原则，设计jar包
    >     
    >     3. 排除jar包：在用``<dependency>```引入jar包时，标签中可以使用```<exclusions>```来排除冲突的jar包
    >     
    > 2. jar包版本锁定
    > 
    >    pom中支持在引入jar包的```<dependencies>```标签前面，使用```<dependencyManagement>```标签对重要的jar包进行版本锁定
    >    
    >    版本锁定后就可以不用怕依赖传递和最短路径导致的jar包版本不一样了
    >    
    >    版本锁定的jar包只是告诉要使用哪个版本jar包，不是引入jar包。
    >    
### 5. 仓库管理
5. 仓库管理
   
    > 1. 参见文档
### 6. 生命周期
6. 生命周期
   
    > 1. 参见文档
### 7. 插件和目标
7. 插件和目标
   
    > 1. 参见文档
### 8. 继承
8. 继承
   
    > 1. 
### 9. 聚合

9. 聚合

   > 1. 
   > 
### 10. ssm父工程需要的pom
10. ssm父工程需要的pom.xml
    > 1. 父工程要保证在pom.xml中，打包方式为pom
    >
    > 2. 父工程的公共依赖
    >
    >     ```
    >     <properties>
    >     		<!-- 公共依赖 -->
    >     		<commons-io.version>2.5</commons-io.version>
    >     		<commons-lang3.version>3.6</commons-lang3.version>
    >     		<commons-codec.version>1.10</commons-codec.version>
    >     		<commons-beanutils.version>1.9.3</commons-beanutils.version>
    >     		<commons-collections.version>3.2.2</commons-collections.version>
    >     		<commons-math3.version>3.6.1</commons-math3.version>
    >     		<commons.fileupload>1.3.2</commons.fileupload>
    >     		<commons-email.version>1.4</commons-email.version>
    >     
    >     		<!-- junit -->
    >     		<junit.version>4.12</junit.version>
    >     		<!-- 时间日期操作 -->
    >     		<joda-time.version>2.9.9</joda-time.version>
    >     		<!-- httpclient -->
    >     		<httpclient.version>4.5.3</httpclient.version>
    >     		<!-- 功能组件 -->
    >     		<poi.version>3.16</poi.version>
    >     		<quartz.version>2.2.3</quartz.version>
    >     		<!-- 数据库 -->
    >     		<druid.version>1.1.0</druid.version>
    >     		<mysql.connector>5.1.42</mysql.connector>
    >     
    >     
    >     		<!-- 基础框架 -->
    >     		<spring.version>4.3.8.RELEASE</spring.version>
    >     		<mybatis.version>3.4.0</mybatis.version>
    >     		<mybatis.spring.version>1.3.1</mybatis.spring.version>
    >     
    >     		<!-- 分页 -->
    >     		<pagehelper.version>5.0.3</pagehelper.version>
    >     		<!-- jackson -->
    >     		<jackson.version>2.7.4</jackson.version>
    >     
    >     		<!-- MBG -->
    >     		<mbg.version>1.3.5</mbg.version>
    >     
    >     		<!-- 日志 -->
    >     		<log4j.version>1.2.17</log4j.version>
    >     		<slf4j.version>1.7.6</slf4j.version>
    >     		<logback.version>1.2.3</logback.version>
    >     		<jcl.over.slf4j>1.7.25</jcl.over.slf4j>
    >     		<jul.to.slf4j>1.7.25</jul.to.slf4j>
    >     		
    >     
    >     		<!-- servlet-api，jsp-api，jstl -->
    >     		<servlet-api.version>2.5</servlet-api.version>
    >     		<jsp-api.version>2.2</jsp-api.version>
    >     		<jstl.version>1.2</jstl.version>
    >     	</properties>
    >     	<!-- 依赖管理 -->
    >     	<dependencyManagement>
    >     		<dependencies>
    >     			<!-- 公共依赖 -->
    >     			<dependency>
    >     				<groupId>commons-io</groupId>
    >     				<artifactId>commons-io</artifactId>
    >     				<version>${commons-io.version}</version>
    >     			</dependency>
    >     			<dependency>
    >     				<groupId>org.apache.commons</groupId>
    >     				<artifactId>commons-lang3</artifactId>
    >     				<version>${commons-lang3.version}</version>
    >     			</dependency>
    >     			<dependency>
    >     				<groupId>commons-codec</groupId>
    >     				<artifactId>commons-codec</artifactId>
    >     				<version>${commons-codec.version}</version>
    >     			</dependency>
    >     			<dependency>
    >     				<groupId>commons-beanutils</groupId>
    >     				<artifactId>commons-beanutils</artifactId>
    >     				<version>${commons-beanutils.version}</version>
    >     			</dependency>
    >     			<dependency>
    >     				<groupId>commons-collections</groupId>
    >     				<artifactId>commons-collections</artifactId>
    >     				<version>${commons-collections.version}</version>
    >     			</dependency>
    >     			<dependency>
    >     				<groupId>org.apache.commons</groupId>
    >     				<artifactId>commons-math3</artifactId>
    >     				<version>${commons-math3.version}</version>
    >     			</dependency>
    >     			<dependency>
    >     				<groupId>commons-fileupload</groupId>
    >     				<artifactId>commons-fileupload</artifactId>
    >     				<version>${commons.fileupload}</version>
    >     			</dependency>
    >     			<dependency>
    >     				<groupId>org.apache.commons</groupId>
    >     				<artifactId>commons-email</artifactId>
    >     				<version>${commons-email.version}</version>
    >     			</dependency>
    >     
    >     			<!--公共依赖结束 -->
    >     
    >     			<!-- junit -->
    >     			<dependency>
    >     				<groupId>junit</groupId>
    >     				<artifactId>junit</artifactId>
    >     				<version>${junit.version}</version>
    >     				<scope>test</scope>
    >     			</dependency>
    >     
    >     			<!-- 时间日期 -->
    >     			<dependency>
    >     				<groupId>joda-time</groupId>
    >     				<artifactId>joda-time</artifactId>
    >     				<version>${joda-time.version}</version>
    >     			</dependency>
    >     
    >     			<!-- httpclient -->
    >     			<dependency>
    >     				<groupId>org.apache.httpcomponents</groupId>
    >     				<artifactId>httpclient</artifactId>
    >     				<version>${httpclient.version}</version>
    >     			</dependency>
    >     
    >     			<dependency>
    >     				<groupId>redis.clients</groupId>
    >     				<artifactId>jedis</artifactId>
    >     				<version>2.9.0</version>
    >     			</dependency>
    >     
    >     			<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
    >     			<dependency>
    >     				<groupId>mysql</groupId>
    >     				<artifactId>mysql-connector-java</artifactId>
    >     				<version>${mysql.connector}</version>
    >     			</dependency>
    >     			<!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
    >     			<dependency>
    >     				<groupId>com.alibaba</groupId>
    >     				<artifactId>druid</artifactId>
    >     				<version>${druid.version}</version>
    >     			</dependency>
    >     
    >     			<!-- 基础框架 -->
    >     			<!-- Spring配置 -->
    >     			<dependency>
    >     				<groupId>org.springframework</groupId>
    >     				<artifactId>spring-beans</artifactId>
    >     				<version>${spring.version}</version>
    >     			</dependency>
    >     			<dependency>
    >     				<groupId>org.springframework</groupId>
    >     				<artifactId>spring-tx</artifactId>
    >     				<version>${spring.version}</version>
    >     			</dependency>
    >     			<dependency>
    >     				<groupId>org.springframework</groupId>
    >     				<artifactId>spring-core</artifactId>
    >     				<version>${spring.version}</version>
    >     			</dependency>
    >     			<dependency>
    >     				<groupId>org.springframework</groupId>
    >     				<artifactId>spring-context</artifactId>
    >     				<version>${spring.version}</version>
    >     			</dependency>
    >     			<dependency>
    >     				<groupId>org.springframework</groupId>
    >     				<artifactId>spring-jdbc</artifactId>
    >     				<version>${spring.version}</version>
    >     			</dependency>
    >     			<dependency>
    >     				<groupId>org.springframework</groupId>
    >     				<artifactId>spring-aspects</artifactId>
    >     				<version>${spring.version}</version>
    >     			</dependency>
    >     			<dependency>
    >     				<groupId>org.springframework</groupId>
    >     				<artifactId>spring-orm</artifactId>
    >     				<version>${spring.version}</version>
    >     			</dependency>
    >     			<dependency>
    >     				<groupId>org.springframework</groupId>
    >     				<artifactId>spring-webmvc</artifactId>
    >     				<version>${spring.version}</version>
    >     			</dependency>
    >     			<dependency>
    >     				<groupId>org.springframework</groupId>
    >     				<artifactId>spring-webmvc-portlet</artifactId>
    >     				<version>${spring.version}</version>
    >     			</dependency>
    >     			<dependency>
    >     				<groupId>org.springframework</groupId>
    >     				<artifactId>spring-test</artifactId>
    >     				<version>${spring.version}</version>
    >     				<scope>test</scope>
    >     			</dependency>
    >     			<dependency>
    >     				<groupId>org.springframework</groupId>
    >     				<artifactId>spring-context-support</artifactId>
    >     				<version>${spring.version}</version>
    >     			</dependency>
    >     
    >     			<!-- mybatis配置 -->
    >     			<dependency>
    >     				<groupId>org.mybatis</groupId>
    >     				<artifactId>mybatis-spring</artifactId>
    >     				<version>${mybatis.spring.version}</version>
    >     			</dependency>
    >     			<dependency>
    >     				<groupId>org.mybatis</groupId>
    >     				<artifactId>mybatis</artifactId>
    >     				<version>${mybatis.version}</version>
    >     			</dependency>
    >     			<!-- 基础框架完成 -->
    >     
    >     			<!-- 分页 -->
    >     			<dependency>
    >     				<groupId>com.github.pagehelper</groupId>
    >     				<artifactId>pagehelper</artifactId>
    >     				<version>${pagehelper.version}</version>
    >     			</dependency>
    >     
    >     			<!-- jackson -->
    >     			<dependency>
    >     				<groupId>com.fasterxml.jackson.core</groupId>
    >     				<artifactId>jackson-core</artifactId>
    >     				<version>${jackson.version}</version>
    >     			</dependency>
    >     			<dependency>
    >     				<groupId>com.fasterxml.jackson.core</groupId>
    >     				<artifactId>jackson-databind</artifactId>
    >     				<version>${jackson.version}</version>
    >     			</dependency>
    >     
    >     			<!-- MBG -->
    >     			<dependency>
    >     				<groupId>org.mybatis.generator</groupId>
    >     				<artifactId>mybatis-generator-core</artifactId>
    >     				<version>${mbg.version}</version>
    >     			</dependency>
    >     
    >     
    >     			<!-- 日志 -->
    >     			<dependency>
    >     				<groupId>log4j</groupId>
    >     				<artifactId>log4j</artifactId>
    >     				<version>${log4j.version}</version>
    >     			</dependency>
    >     			<dependency>
    >     				<groupId>org.slf4j</groupId>
    >     				<artifactId>slf4j-log4j12</artifactId>
    >     				<version>${slf4j.version}</version>
    >     			</dependency>
    >     			<dependency>
    >     				<groupId>org.slf4j</groupId>
    >     				<artifactId>slf4j-api</artifactId>
    >     				<version>${slf4j.version}</version>
    >     			</dependency>
    >     			
    >     			<dependency>
    >     				<groupId>ch.qos.logback</groupId>
    >     				<artifactId>logback-classic</artifactId>
    >     				<version>${logback.version}</version>
    >     			</dependency>
    >     			
    >     			
    >     			<dependency>
    >     		       <groupId>org.slf4j</groupId>
    >     		       <artifactId>jcl-over-slf4j</artifactId><!-- 替换commons-logging-->
    >     		       <version>${jcl.over.slf4j}</version>
    >     		     </dependency>
    >     		     
    >     		     <dependency>
    >     		       <groupId>org.slf4j</groupId>
    >     		       <artifactId>jul-to-slf4j</artifactId><!-- 替换java.util.logging-->
    >     		       <version>${jul.to.slf4j}</version>
    >     		     </dependency>
    >     		     
    >     			
    >     
    >     			<!-- 依赖的WEB类库 -->
    >     			<dependency>
    >     				<groupId>javax.servlet.jsp</groupId>
    >     				<artifactId>jsp-api</artifactId>
    >     				<version>${jsp-api.version}</version>
    >     				<scope>provided</scope>
    >     			</dependency>
    >     			<dependency>
    >     				<groupId>javax.servlet</groupId>
    >     				<artifactId>servlet-api</artifactId>
    >     				<version>${servlet-api.version}</version>
    >     				<scope>provided</scope>
    >     			</dependency>
    >     			<dependency>
    >     				<groupId>javax.servlet</groupId>
    >     				<artifactId>jstl</artifactId>
    >     				<version>${jstl.version}</version>
    >     			</dependency>
    >     
    >     
    >     
    >     		</dependencies>
    >     	</dependencyManagement>
    >     ```