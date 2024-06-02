https://junit.org/junit5/docs/current/user-guide/#running-tests-tags
## @Tag
테스트에 태그를 달 수 있다. 특정 태그를 가진 테스트만 골라 수행할 수 있다. 

```java
    @Test
    @Tag("taxes")
    void testingTaxCalculation() {
    }
```
```java
@Target({ ElementType.TYPE, ElementType.METHOD })
@Retention(RetentionPolicy.RUNTIME)
@Tag("fast")
public @interface Fast {
}
```
## Filtering
### IntelliJ에서 특정 태그가 달린 테스트만 실행하기
https://binux.tistory.com/131
### Gradle
```gradle
test {
    useJUnitPlatform {
        includeTags("fast", "smoke & feature-a")
        // excludeTags("slow", "ci")
        includeEngines("junit-jupiter")
        // excludeEngines("junit-vintage")
    }
}
```
### maven
```
<build>
    <plugins>
        <plugin>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>3.1.2</version>
            <configuration>
                <groups>acceptance | !feature-a</groups>
                <excludedGroups>integration, regression</excludedGroups>
            </configuration>
        </plugin>
    </plugins>
</build>
```
