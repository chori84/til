# Spring Data Redis

## 레디스
- 키밸류 저장소, memcached와 비슷
- 값은 문자열이 될 수 있지만 리스트, 셋, 정렬 셋 지원
- push/pop, add/remove, 서버사이드 결합, 상호작용, set들 간의 차이 가능
- 정렬 지원

## 레이드에 접속
- org.springframework.data.redis.connection
    - RedisConnection
    - RedisConnectionFactory

### RedisConnection, RedisConnectionFactory
- 레디스 백엔드에서 통신을 다룬다.
- 커넥팅 라이브러리 예외를 스프링 일관성 DAO 예외로 전환해준다.
- 네이티브 라이브러리 API가 필요한 경우 getNativeConnection 메소드 사용
- 활성화된 RedisConnection들은 RedisConnectionFactory를 통해서 생성됨.
- 팩토리는 PersistenceExceptionTranslator로 활동
- 적절한 커넥터를 IOC 커넥터를 이용해 설정하고 클래스에 주입한다.

### Jedis
- org.springframework.data.redis.connection.jedis
- 스프링 데이터 레디스가 지원하는 커넥터 중 하나

### JRedis
- 또 다른 유명 오픈소스 커넥터
- org.springframework.data.redis.connection.jredis
- 커넥션 풀을 사용할 수 있다.

### SRP
- 스프링 데이터 레디스의 세번째 오픈소스 커넥터
- org.springframework.data.redis.connection.srp

### Lettuce
- 스프링 데이터 레디스의 네번째 오픈소스 커넥터
- org.springframework.data.redis.connection.lettue
- 논블로킹-논트랜잭션 작업을 위해 같은 쓰레드 세이프한 네이티브 커넥션을 공유한다.
- shareNativeConnection을 false
    - 제공된 커넥션을 매시간 사용
    - 모든 커넥션 사용
- LettucePool
    - pooling blocking
    - transactional한 커넥션 사용

## 레디스 센티널 지원
- RedisSentinelConfiguration 사용
- Jedis만 지원
- properties를 통해 설정 가능
    - spring.redis.sentinel.master : 마스터 노드 이름
    - spring.redis.sentinel.nodes : 콤마로 구분 된 호스트:포트
- RedisConnectionFactory.getSentinelConnection(), RedisConnection.getSentinelCommands() : 활성화 설정된 센티넬 접근 제공

## RedisTemplate을 통해 객체와 작업하기
- org.springframework.data.redis.core
- 레디스 모듈의 중심 클래스
- 높은 레벨의 추사화 제공
- 자바기반 직렬화
    - org.springframework.data.redis.serializer
- 커넥션 관리
- 특정 타입이나 특정 키를 위해 인터페이스 제공
- 스레드 세이프하게 여러 인스턴스에서 재사용

## 문자열 중점 편의 클래스
- StringRedisConnection, StringRedisTemplate
- 레디스에 저장되는 키, 밸류가 java.lang.String인 경우가 흔하기 때문에 제공
- RedisCallback 사용 가능.
- RedisTemplate의 제네릭 타입이 자동으로 String이 되는듯

## 직렬화
- 레디스에 저장된 데이터는 bytes이다.
- RedisSerializer를 통해 유저 타입과 로우 데이터 간의 전환이 다루어진다.
- OxmSerializer(XML), JacksonJsonredisSerializer/Jackson2JsonRedisSerializer(JSON)

## 레디스 메시징
- 메시지 생산, 출판 / 소비, 구독 영역으로 나눠진다. pubsub
- RedisTemplate를 통해 메시지 생산(pub)
- 비동기 : 메시지 리스너 컨테이너 / 동기 : RedisConnection contract

### 메시지 송신/발행
- RedisTemplate 사용
- publish 메소드를 이용해 아규먼트에 보낼 메시지와 목적지 채널을 받는다.
- RedisConnection은 byte배열을 받고, RedisTemplate은 무작위 객체를 메시지로 보낼 수 있다.

### 메세지 수신/구독
- 하나 또는 여러개 채널에서 직접 네이밍하거나 패턴 매칭으로 구독할 수 있다.
- 패턴 매칭으로 여러개의 구독이 하나의 명령어로 생성되고, 구독시간에 생성되지 않은 채널에도 listen을 한다.
- RedisConnection
    - subscribe, pSubscribe 메소드 제공
    - getSubscription, isSubscribed 메소드 제공 : 커넥션 구독이나 단순하게 쿼리를 바꿈
- 구독 명령어는 블로킹이기 때문에 쓰레드가 메시지를 기다리면서 블록된다.
- 구독 해제하거나 다른 쓰레드가 unsubscribe, pUnsubscribe를 하면 해제됨.
- 구독되면 새로운 구독 추가나 현재 구독 수정/취소 외엔 다른 명령어 실행 불가
- 메시지 구독을 위해 MessageListener 콜백을 구현해야 한다.

- 메시지 리스너 컨테이너
    - RedisMessageListenerContainer
    - 레디스 채널에서 메시지를 받는데 사용 된다.
    - 리스너들에게 메시지 dispatch
    - MDP와 메시지 공급자의 중개인
    - 수신된 메시지르 등록하고 자원습득과 해제 예외 전환 등을 관리
    - 커넥션과 쓰레드를 여러 리스너가 공유하게 해준다.
    - 런타임 설정변화 허용해 재시작 없이 리스너 추가 삭제 가능
    - 지연 구독으로 필요할 때만 RedisConnection 사용
    - 동기적 방법을 위해 java.util.concurrent.Executor 필요

- 메시지 리스너 어댑터
    - 비동기 메시지 지원
    - 어떤 클래스든 MDP로 노출 시켜준다.
    - 다양한 message의 타입의 핸들링 메소드가 있다.
    - 레디스 의존성을 가지고 있지 않다.
    - 메시지가 수신되면 RedisSerializer로 타입간 전환을 수행한다.

## 레디스 트랜잭션
- multi, exec, discard 커맨드를 통하여 지원
- RedisTemplate을 통해 사용 가능
- 여러 작업이 같은 connection에서 트랜잭션 필요할 때 사용하는 SessionCallback 인터페이스 제공

### @Transactional 지원
- setEnableTransactionSupport(true) 설정으로 트랜잭션 활성화
- MULTI로 발동, 정상 종료면 EXEC 호출, 에러 발생하면 DISCARD 호출

### 파이프라이닝
- 여러개의 커맨드를 응답 대기 없이 서버로 전송
- 관련 RedisTemplate 메소드 제공
- 파이프라인된 작업 결과를 신경 쓰지 않으면 execute 메소드 사용, pipeline 아규먼트에 true 전달
- executePipelined 메소드는 RedisCallback이나 SessionCallback을 파이프라인에서 실행하고 결과 리턴

## 레디스 스크립팅
- eval, evalsha 명령어 통해 Lua 스크립트 실행 지원
- Redistemplate의 execute 메소드를 통해 실행
- ScriptExecutor를 사용
- 스크립트의 SHA1을 얻고, evalsha를 시도한다.
- 스크립트가 레디스 스크립트 캐쉬에 나타나지 않으면 eval 한다.
- 원자적인 명령어 셋을 실행하는데 한 명령어 실행이 다른 결과에 의해 영향을 받는다.

## 지원 클래스들
- org.springframework.data.redis.support
- atomic 카운터, JDK Collections 같은 인터페이스를 레디스에서 구현
- atomic counter : Redis 키 증가를 쉽게 wrap하게 해준다.
- Collections : 저장소나 API의 노출을 최소한하며, 레디스 키를 쉽게 관리 해준다.
    - RedisSet, RedisZSet : intersection, union - set 작업
    - RedisList : List, Queue, Deque 구현 - FIFO, LIFO, capped collection

### 스프링 캐쉬 추상화 지원
- org.springframework.data.redis.cache
- RedisCacheManager 설정 추가
- Cache 요청 될때마다 지연적으로 RedisCache를 초기화, Set으로 정의
- setTransactionAware로 트랜잭션 지원 활성화