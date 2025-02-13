# Java Redis Client

- java의 Redis 클라이언트에는 크게 2가지가 존재합니다.

  - Jedis , Lettuce

### Redis는 thread-safe 한가?

Redis 서버 자체는 싱글 스레드로 동작하여 요청을 순차적으로 처리하기 때문에, 여러 클라이언트가 동시에 요청을 보내도 내부적으로는 한 번에 하나씩 처리됩니다. 

따라서, Redis 서버 자체는 thread-safe합니다

#### **Redis 클라이언트에서 thread-safety가 문제가 되는 이유는?**

Redis 클라이언트(Java 애플리케이션에서 Redis와 통신하는 라이브러리)는 **멀티 스레드 환경에서 여러 개의 스레드가 하나의 Redis 커넥션을 공유할 경우** thread-safety 문제가 발생할 수 있습니다.



jedis

- 기본적으로 **동기(Blocking) 방식**을 사용합니다. 즉, 하나의 요청을 보낼 때 Redis 서버에서 응답이 올 때까지 해당 스레드는 대기하게 됩니

-  **thread-safe하지 않습니다**. 

- Jedis는 하나의 스레드에서만 안전하게 사용할 수 있도록 설계되었습니다

- 멀티 스레드 환경에서는 `JedisPool`을 사용하여 각 스레드가 **별도의 Jedis 인스턴스**를 가져야 합니다.

  

lettuce

- **비동기(Async) 및 넌블로킹(Non-Blocking) 방식**을 지원하며, Netty를 기반으로 동작합니다. 
  - 따라서 `Future`, `CompletableFuture`, `Reactive Streams`를 활용한 비동기 프로그래밍이 가능합니다.

- 비동기 및 넌블로킹 I/O 기반의 Netty를 사용하며, 기본적으로 **thread-safe**하게 동작합니다. 
- **기본적으로 thread-safe**하기 때문에 **하나의 `RedisConnection` 커넥션을 여러 스레드에서 공유**할 수있습니다.
  - 이는 멀티스레드 환경에서 더 효율적으로 작동할 수 있도록 설계된 것입니다.
  - 트래픽이 많은 환경에서 많은 Jedis보다 효율적입니다.
- Lettuce는 커넥션 관리를 자동으로 처리하여 개발자가 커넥션 풀을 직접 관리할 필요가 없습니다.
