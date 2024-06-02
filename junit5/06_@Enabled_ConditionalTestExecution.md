## ExecutionCondition
컨테이너를 활성/비활성 하거나 프로그램적으로 특정 조건에서만 돌아가도록 하는 확장 API다. 
`@Disabled~`로 애노테이션을 사용할 땐 disabledReason 속성으로 사유를 적어줄 수 있다. 
### @EnableOnOs, @DisableOnOs
특정 운영 체제, 아키텍처에서만 테스트를 수행 또는 예외할 수 있다. 
```java
@EnabledOnOs(MAC)
@TestOnMac
@EnabledOnOs(architectures = "aarch64")
@DisabledOnOs(value = MAC, architectures = "aarch64")
```
### @EnabledOnJre, @DisabledOnJRE, @EnabledForJreRange, @DisabledOnJreRange
특정 JRE 버전에서만 테스트를 수행 또는 예외할 수 있다. 
```java
@EnabledOnJre(JAVA_8)
@EnabledForJreRange(min = JAVA_9, max = JAVA_11)
```
### @EnabledInNativeImage, @DisabledInNativeImage
GraalVM Native Image에서 테스트를 수행 또는 예외할 수 있다. 
https://www.graalvm.org/22.0/reference-manual/native-image/
### @EnabledIfSystemProperty, @DisabledIfSystemProperty
Jvm system 프로퍼티를 조건으로 테스트를 수행 또는 예외할 수 있다. 
```
@DisabledIfSystemProperty(named = "ci-server", matches = "true")
@EnabledIfSystemProperty(named = "os.arch", matches = ".*64.*")
```
### @EnabledIfEnvironmentVariable, @DisabledIfEnvironmentVariable
특정 환경 변수를 조건으로 테스트를 수행 또는 예외할 수 있다. 
```java
@EnabledIfEnvironmentVariable(named = "ENV", matches = "staging-server")
@DisabledIfEnvironmentVariable(named = "ENV", matches = ".*development.*")
```
### @EnabledIf, @DisableIdf
사용자가 정의한 조건에 맞으면 테스트를 수행하거나 예외할 수 있다. 
사용자 정의 조건을 판단하는 메소드의 시그니처는 return type이 boolean, input 매개변수는 없거나 1개의 ExtensionContext 변수여야 한다. 
```java
  @Test
    @EnabledIf("example.ExternalCondition#customCondition")
    void enabled() {
        // ...
    }

```
```java
class ExternalCondition {
    static boolean customCondition() {
        return true;
    }
}
```

