# Spring JPA 수동설정

### 1. DataSource 설정

Hikari DataSource를 구현체로 사용합니다.

application.yaml

```yaml
h2:
  datasource:
    jdbc-url: jdbc:h2:mem:mydb
    username: sa
    password:
    driver-class-name: org.h2.Driver
```

<br>

DatasourceConfig.kt

```kotlin
@Configuration
class H2DataSourceConfiguration {
    
    @Bean(name = ["h2-datasource"])
    @ConfigurationProperties("h2.datasource")
    fun h2DataSource(): DataSource {
        return DataSourceBuilder
            .create()
            .type(HikariDataSource::class.java)
            .build()
    }
}
```

<br>

### 2. EntityManager 설정

LocalContainerEntityManagerFactoryBean 객체를 생성 및 설정합니다.

LocalContainerEntityManagerFactoryBean 객체는 JPA EntityManagerFactory 를 생성하는 Factory Bean 객체입니다.

EntityManager 가 persistence 저장소로 사용할 **DataSource** 를 설정합니다.

EntityManager 가 사용할 persistence 저장소의 이름을 **persistenceUnitName** 값으로 설정합니다.

EntityManager 가 사용할 Entity 클래스들이 위치한 경로를 **setPackagesToScan** 메소드로 명시합니다. 명시한 경로를 통해 **@Entity** 어노테이션이 붙은 객체들을 스캔합니다. 이렇게 찾은 Entity들은 앞으로 EntityManager가 관리할 대상으로 인식됩니다.

EntityManager 가 JPA 구현체로 사용할 **HibernateJpaVendorAdapter** 객체를 생성 및 설정합니다.

EntityManager 가 사용할 JPA 설정을 **setJpaProperties** 메소드로 명시합니다. 

이 때, DataSource와 JpaProperties 값은 컨텍스트에서 이미 정의한 Bean을 사용합니다.

Properties 안에서 쓰이는 설정값들은 아래 예시와 같이 바로 값을 사용할 수도 있지만, **application.yaml 파일** 을 통해 외부화하여 관리할 수도 있습니다. 각 phase 별로 다르게 설정값을 다르게 하고 싶다면, 외부화하는 것이 좋습니다.

<br>

```kotlin
@Configuration
class H2DataSourceConfiguration {
	// dataSource 설정 생략...
	
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
}
```

<br>

### 3. TransactionManager 설정

JpaTransactionManager 객체를 생성합니다.

위에서 설정한 EntityManagerFactory를 설정합니다.

TransactionManager를 반환합니다.

```kotlin
@Configuration
class H2DataSourceConfiguration {
	// DataSource 설정 생략...
	// EntityManager 설정 생략...
	
	@Bean(name = ["h2-transaction-manager"])
    fun transactionManager(
        @Qualifier("h2-entity-manager-factory") entityManagerFactory: EntityManagerFactory
    ): TransactionManager {
        return JpaTransactionManager(entityManagerFactory)
    }
}
```

<br>

### 4. TransactionManagement를 활성화

@EnableTransactionManagement 어노테이션을 이용하여 TransactionManagement 가 이루어지도록 설정합니다. 이 어노테이션을 명시하면, Transaction 관리가 이루어집니다.

이 어노테이션은 Spring Boot에서는 기본적으로 설정되어있는 어노테이션으로 별도 설정하지 않아도 됩니다.

<br>

### 5. Repository 구현체를 생성하도록 명시

@EnableJpaRepositories 어노테이션을 이용하여 Repository 구현체가 자동생성되도록 명시합니다.

@EnableJpaRepositories 어노테이션의 **basePackages** 속성을 이용하여 @Repository 를 클래스를 탐색할 경로를 명시할 수 있습니다.

@Repository 어노테이션이 붙은 클래스를 찾아 SimpleJpaRepository 구현체로 변환합니다.

<br>

### 참고자료

https://blog.marcosbarbero.com/setup-multiple-datasources-with-spring-boot-spring-data-in-pcf/

https://www.petrikainulainen.net/programming/spring-framework/spring-data-jpa-tutorial-part-one-configuration/

https://www.baeldung.com/the-persistence-layer-with-spring-and-jpa