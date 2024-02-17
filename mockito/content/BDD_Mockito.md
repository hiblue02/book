# BDD Mockito
BDD(행동 주도 개발, Behavior Driven Development) 스타일의 given, when, then으로 mocking/stubbing할 수 있게 해주는 라이브러리.

| 메소드명     | original 메소드           | 설명               |
|----------|------------------------|------------------|
| given    | when                   | 조건, 호출할 메소드와 입력값 |
| will 시리즈 | then 시리즈, doReturn 시리즈 | 반환 값             | 
| then     | verify                 | 검증               |

(*) will 시리즈 : will, willReturn, willAnswer, willThrow, willDoNoting, willCallRealMethod
~~~java
import static org.mockito.BDDMockito.*;

 Seller seller = mock(Seller.class);
 Shop shop = new Shop(seller);

 public void shouldBuyBread() throws Exception {
   //given
   given(seller.askForBread()).willReturn(new Bread());

   //when
   Goods goods = shop.buyBread();

   //then
   assertThat(goods, containBread());
 }
~~~
~~~java
 person.ride(bike);
 person.ride(bike);

 then(person).should(times(2)).ride(bike);
 then(person).shouldHaveNoMoreInteractions();
 then(police).shouldHaveZeroInteractions();
~~~
(*) then 다음의 검증 메소드는 [여기](https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/BDDMockito.Then.html#should()) 참고
참고 
- https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/BDDMockito.html
- https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/BDDMockito.Then.html#should()
- https://www.baeldung.com/bdd-mockito
