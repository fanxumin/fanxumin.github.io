---
layout: post
title: "spring boot and mybatis 多数据源配置"
categories: server
---



# 配置方式
在网上搜索了很多种解决方案，最终总结出以下最简便的解决方案：
1.在resources文件夹下，建立mybatis.properties文件
  在文件中写下数据库链接的配置（有几个数据源就写几个配置），如：
  mybatis.ds.url  = your database url
  mybatis.ds.user = your username
  mybatis.ds.pwd  = your password
  
  以上就是一个数据源的配置,这里注意，application.properties中无需加入数据库链接配置
2.对应每个数据源，各自配置一个config类,同时也对应着一个mapper文件夹（该文件夹下的mapper都是访问同一个数据源）
```java
  @Configuration
  @MapperScan(basePackages = "your mapper's packname",
  sqlSessionFactoryRef="sqlSessionFactoryDS")
  @PropertySource("classpath:mybatis.properties")
  public class MybatisDSConfig {
       private @Value("${mybatis.ds.url}") String url_ds;
       private @Value("${mybatis.ds.user}") String user_ds;
       private @Value("${mybatis.ds.pwd}") String pwd_ds;
    
       @Bean(destroyMethod = "close")
       @Primary
       public DataSource dataSourceDS() {
                BasicDataSource dataSource = new BasicDataSource();
                 dataSource.setUrl(url_ds);
                 dataSource.setUsername(user_ds);
                 dataSource.setPassword(pwd_ds);
                 dataSource.setDriverClassName("com.mysql.jdbc.Driver");
                 dataSource.setMaxActive(20);
                 dataSource.setMaxIdle(12);
                 dataSource.setInitialSize(5);
                 dataSource.setMinIdle(1);
                 dataSource.setMaxWait(6000);
                 dataSource.setRemoveAbandoned(true);
                dataSource.setRemoveAbandonedTimeout(180);
                 dataSource.setValidationQuery("SELECT 1");
                 dataSource.setTimeBetweenEvictionRunsMillis(1000 * 60 * 30);
                 return dataSource;
             } 
    
        @Bean
         @Primary
         public SqlSessionFactoryBean sqlSessionFactoryDS(){
             SqlSessionFactoryBean sessionFactory = new SqlSessionFactoryBean();
                 sessionFactory.setDataSource(dataSourceDS());
                 return sessionFactory;
             }   }

```

如上所示，该配置就能被spring读出。特别注意的一点，上面两个方法都有@Primary注释，表示该数据源为主数据源，其他数据源无需该注释。
  
  



