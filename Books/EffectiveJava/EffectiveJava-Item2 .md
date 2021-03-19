# Item2 생성자에 매개변수가 많다면 빌더를 고려하라.
- 정적 팩터리와 생성자에는 선택적 매개변수가 많을때 적절히 대응하기 어렵다는 똑같은 제약이 있다. 
- 보통 선택적 매개변수가 많을때 사용되는 패턴 
	- 점층적 생성자 패턴을 사용한다. 
	-> 매개변수 개수가 많아지면, 클라이언트 코드를 작성하거나 읽기 어렵다.
	-> ( 매개변수 오입력, 가독성 저하 등등  )
	- 자바 빈즈 패턴 ( JavaBeans Pattern) 
		- 매개변수가 없는 생성자로 객체 생성후, Setter를 통해 매개변수를 설정하는 방식
		- -> 객체 생성을 위해 여러개의 메서드를 호출하고, 객체가 완전히 생성되기 전까지 일관성(consistency)이 무너진 상태에 놓이게 된다.
		- -> 클래스를 불변으로 만들수 없다. 
		- -> ( 스레드 안정성을 위해 추가 작업이 필요하다 ) 

## 빌더 패턴 ( Builder Pattern) 
- 클라이언트는 필요한 객체를 직접 만드는 대신, 필수 매개변수만으로 생성자를 호출 하여 빌더 객체를 얻는다. 
- 그 후, 빌더 객체가 제공하는 메서드를 통해 원하는 매개변수를 선택적으로 설정할 수 있다.
- 마지막으로 매개변수가 없는 Build 메서드를 호출해 필요한 객체를 얻는다.

```
NutritionFacts cocaCola = new NutritionFacts.Builder(240, 8).calories(100).sodium(35).carbohydrate(27).build();
```

위 같은 클라이언트 코드는 쓰기 쉽고, 읽기 쉽다.
빌더 패턴은 명명된 선택적 매개변수(named optional parameters) 를 흉내냈다.
- 빌더 패턴은 계층적으로 설계된 클래스와 함께 쓰기 좋다.
	- 각 계층의 클래스에 관련 빌더를 멤버로 정의하자. 

```
public abstract class Pizza { 
	public enum Topping { HAM, MUSHROOM, ONION, PEPPER, SAUSAGE } 
	final Set toppings; 

	abstract static class Builder<T extends Builder<T>> { 
		EnumSet toppings = EnumSet.noneOf(Topping.class); 
		public T addTopping(Topping topping) { 
			toppings.add(Objects.requireNonNull(topping)); 
			return self(); 
		}
		abstract Pizza build(); 
		protected abstract T self(); 
	} 
	Pizza(Builder builder) { 
		toppings = builder.toppings.clone();
	}
}
```

```
NyPizza pizza = new NyPizza.Builder(SMALL)
		.addTopping(SAUSAGE).addTopping(ONION).build(); Calzone calzone = new Calzone.Builder()
		.addTopping(HAM).sauceInside().build();
```


- 빌더 패턴은 유연하다. 
	- 빌더 하나로 여러 객체를 순회하면서 만들 수 있고, 빌더에 넘기는 매개변수에 따라 다른 객체를 만들 수도 있다.


- 단점 
	- 객체 생성전에, 빌더부터 만들어야한다. 
	- 빌더 생성 비용이 크진않지만, 좋지도 않다.
	- 매개변수가 4개 이상이 되어야 값어치를 한다. ( 하지만 보통 api는 시간이 지날수록 증가하게 되어있다. )


- 정리 
	- 생성자나 정적 팩터리가 처리해야 할 매개변수가 많다면 빌더 패턴을 선택하는게 더 낫다. 
	- 매개변수 중 다수가 필수가 아니거나 같은 타입이면 특히 더 그렇다. 빌더는 점층적 생성자보다 클라이언트 코드를 읽고 쓰기가 훨씬 간결하고, 자바 빈즈보다 안전하다.






