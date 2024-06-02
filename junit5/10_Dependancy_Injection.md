## ParameterResolver
ParameterResolver는 런타임 시점에 동적으로 파라미터를 정의한다. (객체를 주입해준다.)
### 생성자 주입
```java

TestInfoDemo(TestInfo testInfo) {
    assertEquals("TestInfo Demo", testInfo.getDisplayName());
}
```
### 메소드 주입
```java
@Test
@DisplayName("TEST 1")
@Tag("my-tag")
void test1(TestInfo testInfo) {
   assertEquals("TEST 1", testInfo.getDisplayName());
   assertTrue(testInfo.getTags().contains("my-tag"));
}
```
## TestInfo, RepetitionInfo, TestReporter
### TestInfo
junit 테스트(메소드, 클래스 단위)에 대한 정보를 갖고 있는 객체 
- getDisplayname
- getTags
- getTestClass
- getTestMethod
https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/TestInfo.html
### RepetitionInfo
테스트 반복에 대한 정보를 갖고 있는 객체, @RepeatedTest, @BeforeEach, and @AfterEach가 붙은 테스트 메소드에 대한 반복 정보를 가진다.
- getCurrentReptition
- getFailureCount
- getFailureThreshold
- getTotalRepetitions
https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/RepetitionInfo.html
### TestReporter
현재 수행된 테스트에 대한 정보를 출력해주는 객체. logger 기능을 한다. 보통 @BeforeEach나 @AfterEach에서 사용한다. (System.out.println보다 가볍다)
```java
 @BeforeEach
 @DisplayName("test1")
 void test296(TestReporter testReporter) {
    testReporter.publishEntry("writer", "hiblue02");
 }
```
`timestamp = 2024-06-02T18:04:11.291366, writer = whale.lee`

https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/TestReporter.html
