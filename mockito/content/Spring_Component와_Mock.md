# Mock 객체를 주입하기
## @InjectMock & @Mock
InjectMock 애노테이션이 선언된 클래스가 가지고 있는 객체에 Mock이 주입된다. (의존성 주입)
(스프링 컴포넌트인 @Service, @repository를 이 방법으로 mocking 할 수 도 있다.)
~~~java
public class MyDictionary {
    Map<String, String> wordMap;

    public MyDictionary() {
        wordMap = new HashMap<String, String>();
    }
    public void add(final String word, final String meaning) {
        wordMap.put(word, meaning);
    }
    public String getMeaning(final String word) {
        return wordMap.get(word);
    }
}
~~~
~~~java
@Mock
Map<String, String> wordMap;

@InjectMocks
MyDictionary dic = new MyDictionary();

@Test
public void whenUseInjectMocksAnnotation_thenCorrect() {
    Mockito.when(wordMap.get("aWord")).thenReturn("aMeaning");

    assertEquals("aMeaning", dic.getMeaning("aWord"));
}
~~~
참고자료 : https://www.baeldung.com/mockito-annotations
# Spring Component를 Mock 객체로 바꿔치기
## @MockBean
Spring applictaion context의 component를 Mock 객체로 대체할 때 애노테이션이다.
~~~java
@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;
    
    public int getUserCount() {
        return userRepository.count();
    }
}

public class MockBeanAnnotationIntegrationTest {
    
    @MockBean
    UserRepository mockRepository;
    
    @Autowired
    Userservice userService;
    
    @Test
    public void givenCountMethodMocked_WhenCountInvoked_ThenMockValueReturned() {
        Mockito.when(mockRepository.count()).thenReturn(123L);
        
        long userCount = userService.getUserCount();

        Assert.assertEquals(123L, userCount);
        // verify : mockRepository에서 count 메소드가 1번 호출됐는지 검증한다.
        Mockito.verify(mockRepository).count(); 
    }
}
~~~
참고문서 : https://www.baeldung.com/java-spring-mockito-mock-mockbean
## @Configuration 이용하기
실제 서비스 대신 Mock 객체를 주입하는 설정 파일을 만든다.
~~~java
@Profile("test") // 설정 이름
@Configuration
public class NameServiceTestConfiguration {
    @Bean
    @Primary // 실제 빈이 있지만, test 설정에서는 아래 내용을 우선 적용한다. (@Primary는 같은 이름의 빈이 여러 개 있을때, 우선적으로 주입할 빈을 표현하는 애노테이션이다.)
    public NameService nameService() {
        return Mockito.mock(NameService.class);
    }
}
~~~

테스트 코드에 위에서 만든 설정을 적용한다. 
~~~java
@ActiveProfiles("test") // 이름으로 설정 불러오기 @ContextConfiguration(classes = NameServiceTestConfiguration.class)을 사용해도 된다.
@SpringApplicationConfiguration(classes = MocksApplication.class)
public class UserServiceUnitTest {

    @Autowired
    private UserService userService;

    // autowired로 빈을 주입하는데, test 설정이 적용되어, NameService는 Mock 객체로 주입된다.
    @Autowired
    private NameService nameService;

    @Test
    public void whenUserIdIsProvided_thenRetrievedNameIsCorrect() {
        // 주입된 NameService(Mock)의 응답값을 설정한다. 
        // (when : 어떤 입력값으로 어떤 메소드를 호출하면, thenReturn: 이 값을 반환한다.)
        Mockito.when(nameService.getUserName("SomeId")).thenReturn("Mock user name");
        String testName = userService.getUserName("SomeId");
        Assert.assertEquals("Mock user name", testName);
    }
}
~~~
참고문서 : https://www.baeldung.com/injecting-mocks-in-spring


