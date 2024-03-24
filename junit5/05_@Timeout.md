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
  - 비동기 작업을 테스트하는 경우: 대부분 `assertTimeoutPreemptively()`와 함께 사용된다.
  - 테스트 간 상호 작용 방지하기 위한 경우
  - ~~~java
    public class TransactionTest {

    @Test
    @Timeout(value = 10, unit = Duration.SECONDS, threadMode = ThreadMode.SEPARATE_THREAD)
    void testAsyncTransaction() {
        // assertTimeoutPreemptively를 사용하여 5초 내에 거래가 완료되는지 확인합니다.
        assertTimeoutPreemptively(Duration.ofSeconds(5), () -> {
            // 거래가 완료될 때까지 기다립니다.
            assertTrue(transactionFuture.get());
        });
    }
    }
    ~~~
