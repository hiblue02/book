# Stubbing (반환 값 셋팅하기)
~~~
when(mock.someMethod("some arg")).thenReturn("one", "two", "three");
~~~
### when
반환 값을 셋팅할 조건(메소드와 입력값)

### thenReturn
반환 값.

| 메소드명 |	설명 |
| - | - |
|thenReturn	|스터빙한 메소드 호출 후 어떤 객체를 반환할 건지 정의 |
|thenThrow	|스터빙한 메소드 호출 후 어떤 예외를 반환할 건지 정의 |
|thenAnswer	|스터빙한 메소드 호출 후 어떤 작업을 할지 커스텀하게 정의(람다식)|
|thenCallRealMethod|실제 메소드 호출 |

~~~java
when(mock.someMethod(anyString())).thenAnswer(
     new Answer() {
         public Object answer(InvocationOnMock invocation) {
             Object[] args = invocation.getArguments();
             Object mock = invocation.getMock();
             return "called with arguments: " + Arrays.toString(args);
         }
 });

 //Following prints "called with arguments: [foo]"
 System.out.println(mock.someMethod("foo"));
~~~ 
### 아무 값이나 셋팅하고 싶다면, Argument Matcher
~~~java
 //stubbing using built-in anyInt() argument matcher
 when(mockedList.get(anyInt())).thenReturn("element");
 //you can also verify using an argument matcher
 verify(mockedList).get(anyInt());
~~~
any~ 시리즈 이외에도, 어떤 문자를 포함한 문자열 등 조건을 넣어 무작위 값을 셋팅할 수 있다.
참고 : https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/ArgumentMatchers.html#any()
### chaging 기능 사용하기
반환 값을 N개 지정하면, 순차적으로 호출된다. N번 이상 호출하면, 가장 마지막에 지정된 반환 값이 호출된다.
~~~java
 when(mock.someMethod("some arg"))
   .thenThrow(new RuntimeException())
   .thenReturn("foo");

 //First call: throws runtime exception:
 mock.someMethod("some arg");

 //Second call: prints "foo"
 System.out.println(mock.someMethod("some arg"));

 //Any consecutive call: prints "foo" as well (last stubbing wins).
 System.out.println(mock.someMethod("some arg"));
~~~
### 리턴 값을 먼저 정의하고 싶다면 doReturn()|doThrow()| doAnswer()|doNothing()|doCallRealMethod() 
~~~java
 doThrow(new RuntimeException()).when(mockedList).clear();

   //following throws RuntimeException:
   mockedList.clear();
~~~
# Verify (검증하기)
| 메소드명                     |	설명 (테스트 내에서) |
|--------------------------| - |
| times(n)	                | 몇 번 호출됐는지 검증|
| never	                   | 한 번도 호출되지 않았는지 검증|
| atLeastOne	              | 최소 한 번은 호출됐는지 검증|
| atLeast(n)	              | 최소 n 번이 호출됐는지 검증|
| atMostOnce	              | 최대 한 번이 호출됐는지 검증|
| atMost(n)	               | 최대 n 번이 호출됐는지 검증|
| only	                    | 해당 검증 메소드만 실행됐는지 검증|
| timeout(long mills)	     |n ms 이상 걸리면 Fail 그리고 바로 검증 종료|
| after(long mills)        |n ms 이상 걸리는지 확인timeout과 다르게 시간이 지나도 바로 검증 종료가 되지 않는다.|
| description	| 실패한 경우 나올 문구 |

~~~java
 //following two verifications work exactly the same - times(1) is used by default
verify(mockedList).add("once");
verify(mockedList, times(1)).add("once");
//verification using never(). never() is an alias to times(0)
verify(mockedList, never()).add("never happened");
//verification using atLeast()/atMost()
verify(mockedList, atMostOnce()).add("once");
verify(mockedList, atLeastOnce()).add("three times");
verify(mockedList, atLeast(2)).add("three times");
verify(mockedList, atMost(5)).add("three times");
//passes as soon as someMethod() has been called 2 times under 100 ms
verify(mock, timeout(100).times(2)).someMethod();
~~~
### InOrder mock이 순차적으로 호출됬는지 확인하기.
~~~java
// B. Multiple mocks that must be used in a particular order
 List firstMock = mock(List.class);
 List secondMock = mock(List.class);

 //using mocks
 firstMock.add("was called first");
 secondMock.add("was called second");

 //create inOrder object passing any mocks that need to be verified in order
 InOrder inOrder = inOrder(firstMock, secondMock);

 //following will make sure that firstMock was called before secondMock
 inOrder.verify(firstMock).add("was called first");
 inOrder.verify(secondMock).add("was called second");

~~~

참고자료 
- https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html
- https://velog.io/@kyukyu/Mockito-%ED%94%84%EB%A0%88%EC%9E%84-%EC%9B%8C%ED%81%AC
