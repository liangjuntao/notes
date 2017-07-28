公司的多个项目，整合方式均不一样。  
为了不懵逼，所以总结下。  

---------------------------------------

mybatis中能打开数据库连接的，能进行增删改查方法的是sqlSession。
1.抽象类org.mybatis.spring.support.SqlSessionDaoSupport提供SqlSession；  
2.org.apache.ibatis.session.SqlSession的实现类org.mybatis.spring.SqlSessionTemplate来获取sqlSession.  
3.采用MapperScannerConfigurer，它将会查找类路径下的映射器并自动将它们创建成MapperFactoryBean。  



其中之一：采用MapperScannerConfigurer  

----------------------------------------------

```
\<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean"\>
	\<property name="dataSource" ref="dataSource" /\>
	\<!-- 自动扫描mapping.xml文件，**表示迭代查找 --\>
	\<property name="mapperLocations" value="classpath:com/hua/saf/**/*.xml" /\>
\</bean\\>
  
<!-- DAO接口所在包名，Spring会自动查找其下的类 ,包下的类需要使用@MapperScan注解,否则容器注入会失败 -->
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="basePackage" value="com.hua.saf.*" />
    <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />
</bean>  
```

编写DAO：其中方法名对应mapper映射中的\<select id=""\>
编写mapper映射：其中namespace对应于<mapper namespace="">DAO中的接口  


其中之一:org.mybatis.spring.SqlSessionTemplate来获取sqlSession.  
------------------------------------------------------------------------
```
<!-- spring和MyBatis完美整合，不需要mybatis的配置映射文件 -->
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
	<property name="dataSource" ref="dataSource" />
	<property name="configLocation"  value="classpath:sqlMapConfig.xml"/>
	<!-- 自动扫描mapping.xml文件，**表示迭代查找,也可在sqlMapConfig.xml中单独指定xml文件-->
	<property name="mapperLocations" value="classpath:com/hua/saf/**/*.xml" />
</bean>
    
	<!-- mybatis spring sqlSessionTemplate,使用时直接让spring注入即可 -->
	<bean id="sqlSessionTemplate" class="org.mybatis.spring.SqlSessionTemplate">
		<constructor-arg index="0" ref="sqlSessionFactory"></constructor-arg>
	</bean>
```

