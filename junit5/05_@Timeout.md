## @Timeout
- 지정된 시간 안에 테스트가 완료되지 않으면 실패로 간주된다.
~~~java
public class TimeoutExample {

    @Test
    @Timeout(value = 5, unit = TimeUnit.SECONDS) // 5초 동안의 타임아웃 설정
    void testWithTimeout() throws InterruptedException {
        // 테스트 내용
        Thread.sleep(6000); // 이 작업이 5초 이상 소요될 경우 타임아웃 발생
    }
}
~~~
## ThreadMode
- `INFERRED`(기본값): junit.jupiter.execution.timeout.thread.mode.default 값으로 실행. 없으면 SAME_THREAD로 실행.
- `SAME_THREAD`: 테스트 메서드가 현재 스레드에서 실행.
- `SEPARATE_THREAD`: 테스트 메서드가 별도의 스레드에서 실행.
  - 비동기 작업을 테스트하는 경우: 비동기 작업이 테스트 되었는지 확인하기 위해
  - 테스트 간 상호 작용 방지하기 위한 경우
  - ~~~java
    public class TransactionTest {

    @Test
    @Timeout(value = 10, unit = Duration.SECONDS, threadMode = ThreadMode.SEPARATE_THREAD)
    void testAsyncTransaction() {
        // 비동기적으로 실행될 거래 코드를 CompletableFuture에 담습니다.
        CompletableFuture<Boolean> transactionFuture = CompletableFuture.supplyAsync(() -> {
            // 비동기 거래 코드
            // 예를 들어, 비동기적으로 데이터베이스에 거래를 기록하고 처리하는 코드 등
            // 이 예제에서는 간단하게 true를 반환하도록 설정했습니다.
            return true;
        });

        // assertTimeoutPreemptively를 사용하여 5초 내에 거래가 완료되는지 확인합니다.
        assertTimeoutPreemptively(Duration.ofSeconds(5), () -> {
            // 거래가 완료될 때까지 기다립니다.
            assertTrue(transactionFuture.get());
        });
    }
    }
    ~~~
