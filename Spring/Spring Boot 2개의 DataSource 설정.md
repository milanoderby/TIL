# Spring Boot 2개의 DataSource 설정

이 글에서는 Kotlin + Spring Boot + Spring Data JPA 환경에서 H2와 MySQL 2개의 DataSource 설정 및 각각의 DataSource로 접근하는 Repository를 구분하는 방법을 알아보겠습니다.

<br>

### 2개의 DataSource 설정

단순히 2개의 DataSource를 설정하는 코드는 아래와 같습니다.

application.yaml

```yaml
h2:
  datasource:
    jdbc-url: jdbc:h2:mem:mydb
    username: sa
    password:
    driver-class-name: org.h2.Driver

mysql:
  datasource:
    jdbc-url: jdbc:mysql://localhost:3306/testdb
    username: root
    password: password
    driver-class-name: com.mysql.cj.jdbc.Driver
```

<br>

DataSourceConfiguration.kt

```kotlin
@Configuration
class MysqlDataSourceConfiguration {
	@Primary
	@Bean(name = ["h2-datasource"])
    @ConfigurationProperties("h2.datasource")
    fun h2DataSource(): DataSource {
        return DataSourceBuilder
            .create()
            .type(HikariDataSource::class.java)
            .build()
    }

	@Bean(name = ["mysql-datasource"])
    @ConfigurationProperties("mysql.datasource")
    fun mysqlDataSource(): DataSource {
        return DataSourceBuilder
            .create()
            .type(HikariDataSource::class.java)
            .build()
    }
}
```

<br>

이렇게 하면, **@Qualifier** 어노테이션을 이용하여 2개의 Datasource를 각각 구분하여 가져올 수 있습니다.

하지만, 위 설정만 하고 그친다면 Spring Data JPA 와 Spring Boot의 자동설정에 의해 **@Primary 어노테이션**이 붙은 DataSource만을 사용하여 TransactionManager 와 EntityManager 등을 생성할 것입니다. 결국, H2 DataSource만 사용될 것입니다.

<br>

이를 방지하기 위해 각각의 DataSource를 이용하여 TransactionManager 와 EntityManager 수동설정을 추가합니다.

Spring JPA 수동설정에 대한 자세한 설정은 이 [문서](https://github.com/milanoderby/TIL/blob/master/Spring/Spring%20JPA%20%EC%88%98%EB%8F%99%EC%84%A4%EC%A0%95.md)를 참고하세요.

결과적으로, 2개의 DataSource 설정은 아래와 같이 설정합니다.

H2DataSourceConfiguration.kt

```kotlin
@Configuration
@EnableJpaRepositories(
    basePackages= ["com.madcoder.jpa_test.domain.country"],
    entityManagerFactoryRef = "h2-entity-manager-factory",
    transactionManagerRef = "h2-transaction-manager"
)
@EnableTransactionManagement
class H2DataSourceConfiguration {
    @Primary
    @Bean(name = ["h2-datasource"])
    @ConfigurationProperties("h2.datasource")
    fun h2DataSource(): DataSource {
        return DataSourceBuilder
            .create()
            .type(HikariDataSource::class.java)
            .build()
    }

    @Primary
    @Bean(name = ["h2-entity-manager-factory"])
    fun h2EntityManagerFactory(
        @Qualifier("h2-datasource") dataSource: DataSource,
        @Qualifier("h2-jpa-properties") properties: Properties
    ): LocalContainerEntityManagerFactoryBean {
        val entityManagerFactoryBean = LocalContainerEntityManagerFactoryBean()
        entityManagerFactoryBean.persistenceUnitName = "h2PersistenceUnit"
        entityManagerFactoryBean.setPackagesToScan("com.madcoder.jpa_test.domain.country")
        entityManagerFactoryBean.dataSource = dataSource
        entityManagerFactoryBean.jpaVendorAdapter = HibernateJpaVendorAdapter()
        entityManagerFactoryBean.setJpaProperties(properties)

        return entityManagerFactoryBean
    }

    @Primary
    @Bean(name = ["h2-jpa-properties"])
    fun properties(): Properties {
        val properties = Properties()
        properties.setProperty(AvailableSettings.DIALECT, "org.hibernate.dialect.H2Dialect")
        properties.setProperty(AvailableSettings.SHOW_SQL, "true")
        properties.setProperty(AvailableSettings.FORMAT_SQL, "true")
        properties.setProperty(AvailableSettings.HBM2DDL_AUTO, "create")
        properties.setProperty(AvailableSettings.PHYSICAL_NAMING_STRATEGY, "org.springframework.boot.orm.jpa.hibernate.SpringPhysicalNamingStrategy")
        properties.setProperty(AvailableSettings.IMPLICIT_NAMING_STRATEGY, "org.springframework.boot.orm.jpa.hibernate.SpringImplicitNamingStrategy")

        return properties
    }

    @Primary
    @Bean(name = ["h2-transaction-manager"])
    fun transactionManager(
        @Qualifier("h2-entity-manager-factory") entityManagerFactory: EntityManagerFactory
    ): TransactionManager {
        return JpaTransactionManager(entityManagerFactory)
    }
}
```

<br>

MysqlDataSourceConfiguration.kt

```kotlin
@Configuration
@EnableJpaRepositories(
    basePackages= ["com.madcoder.jpa_test.domain.member"],
    entityManagerFactoryRef = "mysql-entity-manager-factory",
    transactionManagerRef = "mysql-transaction-manager"
)
@EnableTransactionManagement
class MysqlDataSourceConfiguration {
    @Bean(name = ["mysql-datasource"])
    @ConfigurationProperties("mysql.datasource")
    fun mysqlDataSource(): DataSource {
        return DataSourceBuilder
            .create()
            .type(HikariDataSource::class.java)
            .build()
    }

    @Bean(name = ["mysql-entity-manager-factory"])
    fun mysqlEntityManagerFactory(
        @Qualifier("mysql-datasource") dataSource: DataSource,
        @Qualifier("mysql-jpa-properties") properties: Properties
    ): LocalContainerEntityManagerFactoryBean {
        val entityManagerFactoryBean = LocalContainerEntityManagerFactoryBean()
        entityManagerFactoryBean.persistenceUnitName = "mysqlPersistenceUnit"
        entityManagerFactoryBean.setPackagesToScan("com.madcoder.jpa_test.domain.member")
        entityManagerFactoryBean.dataSource = dataSource
        entityManagerFactoryBean.jpaVendorAdapter = HibernateJpaVendorAdapter()
        entityManagerFactoryBean.setJpaProperties(properties)

        return entityManagerFactoryBean
    }

    @Bean(name = ["mysql-jpa-properties"])
    fun properties(): Properties {
        val properties = Properties()
        properties.setProperty(AvailableSettings.DIALECT, "org.hibernate.dialect.MySQL8Dialect")
        properties.setProperty(AvailableSettings.SHOW_SQL, "true")
        properties.setProperty(AvailableSettings.FORMAT_SQL, "true")
        properties.setProperty(AvailableSettings.HBM2DDL_AUTO, "create")
        properties.setProperty(AvailableSettings.PHYSICAL_NAMING_STRATEGY, "org.springframework.boot.orm.jpa.hibernate.SpringPhysicalNamingStrategy")
        properties.setProperty(AvailableSettings.IMPLICIT_NAMING_STRATEGY, "org.springframework.boot.orm.jpa.hibernate.SpringImplicitNamingStrategy")

        return properties
    }

    @Bean(name = ["mysql-transaction-manager"])
    fun transactionManager(
        @Qualifier("mysql-entity-manager-factory") entityManagerFactory: EntityManagerFactory
    ): TransactionManager {
        return JpaTransactionManager(entityManagerFactory)
    }
}
```

<br>

**@EnableJpaRepositories** 어노테이션 설정을 통해 각 도메인별로 사용할 DataSource, EntityManager, TransactionManager 를 다르게 설정해주었다는 것이 중요합니다.