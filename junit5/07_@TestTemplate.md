 등록된 provider가 context에 있는 값을 활용해 test를 여러번 실행시켜주는 기능이다. 
대표적으로 @ParamarizedTest, @RepeatedTest가 있다. 

### Context Provider 만들기 
#### TestTemplateInvocationContextProvider 인터페이스를 구현한다.
테스트 ContextProvider의 기능을 구현한다. 
#### supportsTestTemplate
junit 실행엔진이 provider가 주어진 ExecutionContext에 해당하는지 검증하는데 사용된다. 
#### provideTestTemplateInvocationContexts
테스트에 사용될 context 인스턴스를 Stream 형태로 반환한다. (쉽게 말해 테스트 input 값 배열)

```java
@Override
public Stream<TestTemplateInvocationContext> provideTestTemplateInvocationContexts(
  ExtensionContext extensionContext) {
    boolean featureDisabled = false;
    boolean featureEnabled = true;
 
    return Stream.of(
      featureDisabledContext(
        new UserIdGeneratorTestCase(
          "Given feature switch disabled When user name is John Smith Then generated userid is JSmith",
          featureDisabled,
          "John",
          "Smith",
          "JSmith")),
      featureEnabledContext(
        new UserIdGeneratorTestCase(
          "Given feature switch enabled When user name is John Smith Then generated userid is baelJSmith",
          featureEnabled,
          "John",
          "Smith",
          "baelJSmith"))
    );
}
```
#### TestTemplateInvocationContext
테스트에 사용될 context 인스턴스이다. TestTemplateInvocationContextProvider.provideTestTemplateInvocationContexts의 반환값이기도 하다. 
TestTemplateInvocationContext도 커스터마이징 할 수 있다. (인터페이스이니, 구현하면 된다 implements)
- getDisplayName
    - displayName을 어떻게 보여줄지 설정하는 부분이다. (인스턴스 각각의 @DisplayName 속성을 지정한다고 보면 된다.)
- getAdditionalExtensions
    - 추가 기능을 붙이는 부분이다. (테스트 실행 전후에 수행될 기능이나, parameter resolver를 붙일 수도 있다)
    - GenericTypedParameterResolver
    - BeforeTestExecutionCallback
    - AfterTestExecutionCallback 
```java
private TestTemplateInvocationContext featureDisabledContext(
  UserIdGeneratorTestCase userIdGeneratorTestCase) {
    return new TestTemplateInvocationContext() {
        @Override
        public String getDisplayName(int invocationIndex) {
            return userIdGeneratorTestCase.getDisplayName();
        }

        @Override
        public List<Extension> getAdditionalExtensions() {
            return asList(
              new GenericTypedParameterResolver(userIdGeneratorTestCase), 
              new BeforeTestExecutionCallback() {
                  @Override
                  public void beforeTestExecution(ExtensionContext extensionContext) {
                      System.out.println("BeforeTestExecutionCallback:Disabled context");
                  }
              }, 
              new AfterTestExecutionCallback() {
                  @Override
                  public void afterTestExecution(ExtensionContext extensionContext) {
                      System.out.println("AfterTestExecutionCallback:Disabled context");
                  }
              }
            );
        }
    };
}
```


### Context Provider를 이용해 테스트 돌리기 
#### @TestTemplate
테스트 메소드에 테스트 템플릿을 사용하겠다고 마킹한다. 
#### @ExtendWith
테스트에 사용할 context provider를 선언한다. 
```java
 public class SampleTest {
    @TestTemplate
    @ExtendWith(UserIdGeneratorTestInvocationContextProvider.class)
    public void whenUserIdRequested_thenUserIdIsReturnedInCorrectFormat(UserIdGeneratorTestCase testCase) {

        UserIdGenerator userIdGenerator = new UserIdGeneratorImpl(testCase.isFeatureEnabled());

        String actualUserId = userIdGenerator.generate(testCase.getFirstName(), testCase.getLastName());

        assertThat(actualUserId).isEqualTo(testCase.getExpectedUserId());
    }
```

참고: 
- https://www.baeldung.com/junit5-test-templates
- https://junit.org/junit5/docs/current/user-guide/#extensions-test-templates




