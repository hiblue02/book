## Assertions
https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/Assertions.html
https://www.baeldung.com/junit-assertions
- assertEqual(expected, actual): actual이 expected와 같은지 확인한다.
- assertNotEqual(expected, actual): actual이 expected와 다른지 확인한다.
- assertSame
- assertNotNull(Object actual): actual이 null이 아닌지 확인한다.
- assertNull(Object actual)
- assertAll(executables ...): executables가 exception을 던지지 않는지 확인한다.
- assertArrayEquals(Array[] expected, Array[] actual): actual과 expected 배열이 같은지 확인한다.
- assertThrows(Class<T> expectedType, Executable executable): executable이 기대한 Exception을 던지는지 확인한다.
- assertDoesNotThrow(executable): executable이 Exception을 던지지 않는지 확인한다.
- assertThrowsExactly(Class<T> expectedType, Executable executable): executable이 기대한 Exception을 정확히 던지는지 확인한다. (상속 허용 안함)
- assertTrue(expected): expected가 boolean True인지 확인한다.
- assertFalse(expected): expected가 boolean False인지 확인한다.
- assertInstanceOf(Class<T> expectedType, Object Actual): Actual이 기대한 타입인지 확인한다.
- assertIterableEquals(Iterable<?> expected, Iterable<?> actual): Iterable 일때
- assertLinesMatch(List<String> expectedLines, List<String> actualLines): list일때
- assertTimeout(Duration timeout, Executable executable): executable이 동작한 시간이 기대한 시간 이내인지 확인한다.
- assertTimeoutPreemptively(Duration timeout, Executable executable): executable이 기대한 시간 안에 동작하는지 확인한다. 
- fail(): 테스트를 실패시킨다. (테스트가 실패했을 경우를 나타내기 위해 사용한다.)
  ```java
    @Test
    public void whenCheckingExceptionMessage_thenEqual() {
        try {
            methodThatShouldThrowException();
            fail("Exception not thrown");
        } catch (UnsupportedOperationException e) {
            assertEquals("Operation Not Supported", e.getMessage());
        }
    }
  ```
### assertEquals VS assertSame
assertEquals: equals나 ==을 이용해 객체가 같은지 비교한다. 
assertSame: 객체의 참조값(주소)이 같은지 비교한다.
https://www.baeldung.com/java-assertequals-vs-assertsame

### assertTimeOut VS assertTimeoutPreemptively
assertTimeOut: 기대한 시간 안에 동작하는지 확인한다.  (테스트가 끝날 때까지 기다린 후 기대한 시간과 동작 시간을 비교한다.)
assertTimeoutPreemptively: 기대한 시간 안에 동작하는지 확인한다. (기대한 시간이 지나면 테스트를 종료한다.)
https://sas-study.tistory.com/318

## Assumption
어떤 조건에서 테스트를 abort(중단)하고 싶을 때 사용한다.
```java
    @Test
    void testOnlyOnDeveloperWorkstation() {
        assumeTrue("DEV".equals(System.getenv("ENV")),
            () -> "Aborting test: not on developer workstation");
        // remainder of test
    }

    @Test
    void testInAllEnvironments() {
        assumingThat("CI".equals(System.getenv("ENV")),
            () -> {
                // perform these assertions only on the CI server
                assertEquals(2, calculator.divide(4, 2));
            });

        // perform these assertions in all environments
        assertEquals(42, calculator.multiply(6, 7));
    }
```
https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/Assumptions.html
- abort(): 테스트를 그냥 중단한다.
- assumFalse(): 어떤 조건이 아니면 테스트를 중단한다.
- assumtTrue(): 어떤 조건이 맞으면 테스트를 중단한다
- assumingThat(boolean assumption, Executable executable): 어떤 조건이면 Executable를 실행한다. 조건이 맞지 않으면 테스트를 중단한다. 
