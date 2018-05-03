# Configuring and Running a Job

## Configuring a Job

- Job인터페이스 의 여러 구현이 있지만 빌더는 구성의 차이점을 추상화한다.

```java
@Bean
public Job footballJob() {
    return this.jobBuilderFactory.get("footballJob")
                     .start(playerLoad())
                     .next(gameLoad())
                     .next(playerSummarization())
                     .end()
                     .build();
}
```

- Job(그리고 일반적으로 그 안에 있는 Step)은 JobRepository가 필요하다.
- JobRepository의 설정은 BatchConfigurer를 통해 다룬다.
- 위 Job은 세 개의 Step으로 이루어져 있다.
- 작업 관련 빌더는 병렬화 ( Split), 선언적 플로우 제어 ( Decision) 및 플로우 정의 ( Flow)의 외부화에 도움이되는 다른 요소를 포함 할 수도 있다.

### Restartability
- 배치 작업을 실행할 때 발생하는 주요 문제 중 하나는 Job이 다시 시작될 때의 동작이다.
- Job의 시작은 특정 JobInstance를 위한 JobExecution가 이미 존재 하는 경우 '재시작'으로 간주된다.
- 이상적으로 모든 작업은 중단 된 부분부터 시작할 수 있어야하지만 불가능한 경우가 있다.
- 이 시나리오에서 새로운 JobInstance가 생성 되도록하는 것은 전적으로 개발자의 몫이다.
- Job을 절대 다시 시작해서는 안되지만, 항상 새로운 JobInstance의 일부로 실행해야하는 경우 restartable 속성을 'false'로 설정할 수 있습니다.


## Java Config
- Spring3은 XML 외에도 Java를 통해 애플리케이션을 구성 할 수있는 기능을 제공한다.
- Spring Batch 2.2.0부터, 배치 작업은 동일한 Java 기반 설정을 사용하여 설정할 수 있다.
- Java 기반 설정은 두가지로 구성 되있다. : @EnableBatchProcessing 어노테이션과 두 가지 빌더.
- @EnableBatchProcessing은 Spring 패밀리의 다른 @Enable* 어노테이션과 비슷하게 작동한다.
- 이 경우 @EnableBatchProcessing은 배치 작업을 빌드하기위한 기본 구성을 제공한다.
- 이 기본 구성 안에서 autowired 할 수 있는 여러 빈들 이외에도 StepScope 인스턴스가 생성된다.
    - JobRepository - bean 명 "jobRepository"
    - JobLauncher - bean 이름 "jobLauncher"
    - JobRegistry - bean 명 "jobRegistry"
    - PlatformTransactionManager - bean 명 "transactionManager"
    - JobBuilderFactory - bean 이름 "jobBuilders"
    - StepBuilderFactory - bean 이름 "stepBuilders"
- 이 설정의 핵심 인터페이스는 BatchConfigurer 이다.
- 기본 구현은 위에서 언급 한 bean을 제공하고, 제공 할 컨텍스트 내의 bean으로서 DataSource가 필요하다.
- 이 데이터 소스는 JobRepository에서 사용합니다.
- 하나의 Configuration 클래스만 @EnableBatchProcessing 어노테이션을 가져야한다.
- 일단 이 어노테이션 달린 클래스가 하나가 있으면, 위의 내용 모두를 이용할 수 있다.
- 기본 구성을 제자리에두면 사용자는 제공된 빌더 팩토리를 사용하여 작업을 구성 할 수 있다.
- 다음은 JobBuilderFactory와 StepBuilderFactory를 통해 설정된 2 step job의 예이다.

```java
@Configuration
@EnableBatchProcessing
@Import(DataSourceConfiguration.class)
public class AppConfig {

    @Autowired
    private JobBuilderFactory jobs;

    @Autowired
    private StepBuilderFactory steps;

    @Bean
    public Job job(@Qualifier("step1") Step step1, @Qualifier("step2") Step step2) {
        return jobs.get("myJob").start(step1).next(step2).build();
    }

    @Bean
    protected Step step1(ItemReader<Person> reader,
                         ItemProcessor<Person, Person> processor,
                         ItemWriter<Person> writer) {
        return steps.get("step1")
            .<Person, Person> chunk(10)
            .reader(reader)
            .processor(processor)
            .writer(writer)
            .build();
    }

    @Bean
    protected Step step2(Tasklet tasklet) {
        return steps.get("step2")
            .tasklet(tasklet)
            .build();
    }
}
```

## Configuring a JobRepository
- @EnableBatchProcessing를 사용하는 경우, JobRepository는 당신을 위해 상자 밖에서 제공된다.
(JobRepository가 자동으로 bean으로 등록된다는 얘기인가?)
- 앞에서 설명한 것처럼, JobRepository는 Spring Batch 내의 JobExecution, StepExecution 같은
다양한 persisted domain objects의 기본 CRUD 명령에 사용된다.
- 그것은 같은 JobLauncher, Job, Step 깉은 주요 프레임 워크의 많은 기능에 의해 필요하다.
- Java 구성을 사용할 때, JobRepository가 제공된다.
- DataSource가 제공되면, JDBC 기반의 것이 제공되고 Map 기반의 것은 제공되지 않는다.
- 그러나 BatchConfigurer 인터페이스를 구현해서 JobRepository의 설정을 커스터마이징 할 수 있다.

```java
...
// This would reside in your BatchConfigurer implementation
@Override
protected JobRepository createJobRepository() throws Exception {
    JobRepositoryFactoryBean factory = new JobRepositoryFactoryBean();
    factory.setDataSource(dataSource);
    factory.setTransactionManager(transactionManager);
    factory.setIsolationLevelForCreate("ISOLATION_SERIALIZABLE");
    factory.setTablePrefix("BATCH_");
    factory.setMaxVarCharLength(1000);
    return factory.getObject();
}
...
```
- 위에 나열된 구성 옵션은 dataSource 및 transactionManager를 제외하고는 필요하지 않다.
- 값을 설정하지 않으면 위의 기본값이 사용된다.
- 위의 내용은 인식의 목적으로 사용되었다.
- max varchar 길이의 기본값은 2500이다. 이것은 샘플 스키마 스크립트에서 긴 VARCHAR 컬럼의 길이이다.

### Transaction Configuration for the JobRepository
- 네임 스페이스 또는 제공된 FactoryBean을 사용하면, transactional advice가 저장소 주위에 자동으로 생성된다.
- 이는 실패 후에 다시 시작하는 데 필요한 상태를 포함하여 일괄 처리 메타 데이터가 올바르게 유지되도록하기위한 것이다.
- repository 메소드가 트랜잭션널하지 않으면 프레임 워크의 동작이 잘 정의되어 있지 않다.
- create* 메소드 속성 중에 격리 수준은 job이 시작될 때 두 프로세스가 동일한 job을 동시에 시작하려고 시도 할 때 하나만 성공할 수 있도록 별도로 지정된다.
- 해당 메소드의 기본 격리 수준은 SERIALIZABLE이며 매우 공격적입니다. READ_COMMITTED도 마찬가지로 작동한다.
- 이런 식으로 두 프로세스가 충돌하지 않으면 READ_UNCOMMITTED가 좋을 것이다.
- 그러나, create* 메소드가 매우 짧기 때문에 데이터베이스 플랫폼이 지원하는 한 SERIALIZED가 문제를 일으킬 가능성은 거의 없다.
- 그러나 이를 재정의 할 수 있다.

```java
// This would reside in your BatchConfigurer implementation
@Override
protected JobRepository createJobRepository() throws Exception {
    JobRepositoryFactoryBean factory = new JobRepositoryFactoryBean();
    factory.setDataSource(dataSource);
    factory.setTransactionManager(transactionManager);
    factory.setIsolationLevelForCreate("ISOLATION_REPEATABLE_READ");
    return factory.getObject();
}
```

- 네임 스페이스 또는 팩토리 빈이 사용되지 않으면 AOP를 사용하여 저장소의 트랜잭션 동작을 설정하는 것이 필수적이다.

```java
@Bean
public TransactionProxyFactoryBean baseProxy() {
        TransactionProxyFactoryBean transactionProxyFactoryBean = new TransactionProxyFactoryBean();
        Properties transactionAttributes = new Properties();
        transactionAttributes.setProperty("*", "PROPAGATION_REQUIRED");
        transactionProxyFactoryBean.setTransactionAttributes(transactionAttributes);
        transactionProxyFactoryBean.setTarget(jobRepository());
        transactionProxyFactoryBean.setTransactionManager(transactionManager());
        return transactionProxyFactoryBean;
}
```

### Changing the Table Prefix
- JobRepository의 수정 가능한 또 다른 속성은 메타 데이터 테이블의 테이블 접두사이다. 
- 기본적으로 모두 BATCH_가 붙는다.
- BATCH_JOB_EXECUTION 및 BATCH_STEP_EXECUTION은 두 가지 예이다.
- 그러나이 접 두부를 수정할 잠재적 인 이유가 있다.
- 스키마 이름을 테이블 이름 앞에 붙여야하거나 동일한 스키마 내에 둘 이상의 메타 데이터 테이블 세트가 필요한 경우 테이블 접두사를 변경해야한다.

```java
// This would reside in your BatchConfigurer implementation
@Override
protected JobRepository createJobRepository() throws Exception {
    JobRepositoryFactoryBean factory = new JobRepositoryFactoryBean();
    factory.setDataSource(dataSource);
    factory.setTransactionManager(transactionManager);
    factory.setTablePrefix("SYSTEM.TEST_");
    return factory.getObject();
}
```

- 위의 변경 사항이 주어지면 메타 데이터 테이블에 대한 모든 쿼리의 접두사는 "SYSTEM.TEST_" 이다.
- BATCH_JOB_EXECUTION은 SYSTEM.TEST_JOB_EXECUTION이 된다.
- 테이블 접두어 만 구성 가능하다. 테이블 및 컬럼 이름은 그렇지 않다.

### In-Memory Repository
- 도메인 개체를 데이터베이스에 저장하지 않으려는 시나리오가 있다.
- 한 가지 이유는 속도 일 수 있다.
- 각 커밋 시점에서 도메인 객체를 저장하는 데는 추가 시간이 필요하다.
- 또 다른 이유는 특정 직업에 대한 status를 유지할 필요가 없다는 것이다.
- 이런 이유로 Spring 배치는 job 저장소의 in-memory Map 버전을 제공한다.

```java
// This would reside in your BatchConfigurer implementation
@Override
protected JobRepository createJobRepository() throws Exception {
    JobRepositoryFactoryBean factory = new JobRepositoryFactoryBean();
    factory.setDataSource(dataSource);
    factory.setTransactionManager(transactionManager);
    factory.setIsolationLevelForCreate("ISOLATION_REPEATABLE_READ");
    return factory.getObject();
}
```

- 메모리 내 저장소는 휘발성이므로 JVM 인스턴스간에 다시 시작할 수 없다.
- 또한 동일한 매개 변수를 갖는 두 개의 job 인스턴스가 동시에 시작되는 것을 보장 할 수 없고,
멀티 스레드 job 또는 locally partitioned Step으로 사용하기에 적합하지 않다.
- 그러나 저장소에 롤백 의미론이 있고 비즈니스 로직이 여전히 트랜잭셔널(예: RDBMS 액세스) 할 수 있으므로 트랜잭션 관리자를 정의해야 한다.
- 테스트 목적으로 많은 사람들이 ResourcelessTransactionManager가 유용하다고 생각합니다.

### Non-standard Database Types in a Repository
- 지원되는 플랫폼 목록에없는 데이터베이스 플랫폼을 사용하는 경우 SQL 변형이 충분히 가까울 경우 지원되는 유형 중 하나를 사용할 수 있다.
- 이렇게하려면 네임 스페이스 바로 가기 대신 JobRepositoryFactoryBean을 그대로 사용하여 가장 일치하는 데이터베이스 type으로 설정한다.

```java
// This would reside in your BatchConfigurer implementation
@Override
protected JobRepository createJobRepository() throws Exception {
    JobRepositoryFactoryBean factory = new JobRepositoryFactoryBean();
    factory.setDataSource(dataSource);
    factory.setDatabaseType("db2");
    factory.setTransactionManager(transactionManager);
    return factory.getObject();
}
```

- (지정되지 않은 경우, JobRepositoryFactoryBean은 DataSource에서 데이터베이스 유형을 자동으로 감지한다.)
- 플랫폼 간의 주요 차이점은 주로 기본 키를 증가시키기 위한 전략에 의해 설명되므로 종종 incrementerFactory도 재정의해야 할 수도 있다.
- 그래도 작동하지 않거나 RDBMS를 사용하지 않는다면, 유일한 방법은 일반적인 스프링 방식으로 SimpleJobRepository를 수동으로 의존하는
다양한 Dao 인터페이스를 구현하는 것이다.
