Item 1. 생성자 대신 정적 팩터리 메서드를 고려하라
	- 장점
		- 이름을 가질수 있다. 
			- ( BigInteger(int, int, Random) 를 사용하는 것보다, BigInteger. probablePrime를 사용하여 객체를 생성하는게, 가독성이 좋아진다.
		- 호출될때 마다 인스턴스를 새로 생성하지는 않아도 된다.
			- 불변 클래스는 인스턴스를 미리 만들어 놓거나, 생성된 인스턴스를 재활용할 수 있다.
			- 대표적인예로 ( Boolean.valueOf(boolean) 메서드는 객체를 생성하지 않는다. 
			- 생성 비용이 큰 객체가 자주 요청되는 상황이라면 성능향상에 도움이된다.
			- 언제 어느 인스턴스를 살아있게 할지를 통제하는 인스턴스 통제 클래스 - 싱글턴/인스턴스화 불가/인스턴스 유일성 
		- 반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.
			- 유연성을 제공하며. 구현 클래스를 공개하지 않고도, 객체를 반환할 수 있어 Api를 작게 유지할수 있다.   -> 인터페이스 기반 프레임워크를 만드는 핵심 기술 
			- 자바 컬렉션 프레임워크는 45개의 유틸리티 구현체를 제공하지만, 단 하나의 Collections에서 정적 팩터리 메서드를 통해 얻는다.
		- 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.
			- 반환 타입의 하위 타입 이기만 하면, 어떤 클래스의 객체를 반환하든 상관없다.
			- ex> EnumSet 클래스는 public 생성자 없이, 오히려 정적 팩터리만 제공하고, 원소의 수에 따라 두가지 하위 클래스중 하나의 인스턴스를 반환한다. (원소 64개 이하  long변수하나로 관리하는 RegularEnumSet / 65개 이상 long배열로 관리하는 JumboEnumSet 반환) 
하지만, 클라이언트는 이 두클래스의 존재를 모른다. EnumSet의 하위 클래스이기만 하면 된다.
		- 정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.
			- 이런 유연함은 서비스 프레임워크를 만드는 근간이 된다. ( ex> jdbc)


- 단점
	- 상속을 하려면 public이나 protected 생성자가 필요하니, 정적 팩터리 메서드만 제공하면, 하위 클래스를 만들수 없다.
	- 정적 팩터리 메서드는 프로그래머가 찾기 어렵다.
		- 생성자처럼 api 설명에 명확히 드러나지 않으니, 사용자는 정적 팩터리 메서드 방식 클래스를 인스턴스화할 방법을 알아내야한다. 


- 흔히 사용하면 메서드
	- from : 매개변수를 하나 받아서 해당 타입의 인스턴스를 반환하는 형변환 메서드 
ex> Date d = Date.from(instant);
	- of : 여러 매개변수를 받아 적함한 타입의 인스턴스를 반환하는 집계 메서드
ex> Set<Rank> faceCards = EnumSet.of(Jack,Queen,King)
	- valueOf : from 과 of의 더 자세한 버전
ex> BigInteger prime = BigInteger.valueOf(Integer.MAX_VALUE)
	- instance or getInstance : (매개변수를 받는다면 ) 매개변수로 명시한 인스턴스를 반환하지만, 같은 인스턴스임을 보장하지않는다.
ex> StackWalker luke = StackWalker.getInstance(options)
	- create or newInstance : instance 혹은 getInstance와 같지만, 매번 새로운 인스턴스를 생성해 반환함을 보장한다.
ex> Object newArray = Array.newInstance(classObject, arrayLen );
	- getType : getInstance와 같으나, 생성할 클래스가 아닌 다른 클래스에 팩터리 메서드를 정의할때 사용한다.  “Type”은 팩터리 메서드가 반환할 객체의 타입니다.
ex> FileStore fs = Files.newBufferedReader(path);
	- newType : newInstance와 같다 ( = getType)
ex> BufferedReader br = Files.newBufferedReader(path);
	- type : getType,newType의 간결한 버전
ex> List<Complaint> litany = Collections.list(legacyLinity);


정리 
정적 팩터리 메서드와 public 생성자는 각자의 쓰임새가 있으니 상대적인 장단점을 이해하고 사용하는것이 좋다. 그렇다고 하더라도, 정적팩터리를 사용하는게 유리한 경우가 더 많으므로 무작정 public 생성자를 제공하던 습관이 있다면 고치는게 좋다.