# CleanCode 2장 의미 있는 이름
## 의도를 분명히 밝혀라
- 변수/함수/클래스 이름에는 존재 이유/수행 기능/사용 방법이 있어야 한다.
	- 따로 주석이 필요하다면, 의도를 분명히 드러내지 못했다는 말이다.


* 나쁜 예
	* int d;
* 좋은 예
	* int elapsedTimeInDays;
	* int daysSinceCreation;
	* int daysSinceModification;
	* int fileAgeInDays;

- 의도가 드러나는 이름을 사용하면, 코드 이해와 변경이 쉬워진다. 

#### 나쁜 예  
```
public List<int[]> getThem() {
    List<int[]> list1 = new ArrayList<int[]>();
    for (int[] x : theList) {
        if (x[0] == 4) {
            list1.add(x);
        }
    }
    return list1;
}
```
- theList는 무엇이 들어있는지, 0번째 값이 왜 중요한지, 값 4는 무슨 의미인지, getThem의 리턴값은 무엇을 의미하는지 
- 코드의 함축성이 문제가 된다. 
- 코드 맥락이 코드 자체에 명시적으로 들어나지 않는다.
	
#### 좋은 예
	
```
public List<Cell> getFlaggedCells() {
    List<Cell> flaggedCells = new ArrayList<Cell>();
    for (Cell cell : gameBoard) 
       if (cell.isFlagged())
          flaggedCells.add(cell);   
    return flaggedCells;
}
```
- 단순히 이름만 바꿔도, 함수가 하는일을 이해할 수 있다.

---
## 그릇된 정보를 피하라
- 널리 쓰이는 의미있는 단어를 다른 단어로 사용하면 안된다. 
- ex> 개발자에게 List는 특수한 의미이다. 실제 List 가 아니라면, accountList라고 명명하지 않는다. ( -> accountGroup, brunchOfAccount, accounts )
- 비슷한 이름을 사용하지 않도록 주의하자 ( 우리는 자동완성 기능을 사용하기 때문에, 다른 모듈에서 비슷한 이름을 사용하면 알기 힘들다.) 

- 나쁜 예 ( i,L,1,0,o ... )
```
int a= 1;
if ( O==1 )
a = O1;
eslse
1 = 01;
```



---

## 의미 있게 구분하라
- 의미 없는 단어를 사용하지마라
	- a1,a2,a3,,, ( 숫자를 덧붙이는 불용어 )

나쁜 예
```
public static void copyChars(char a1[]. char a2[]) { 
	for (int i =0; i < a1.length; i++){
		a2[i] = a1[i]
	}
}
// 함수 인수 이름을 source와 destination으로만 바꿔도 좋다
```

- ProductInfo, ProductData , Product 라고 이름을 짓는다면, 개념을 구분하지 않은 채 이름만 달리한 경우이다. 
	- info, data 는 a , an , the 와 마찬가지로 의미가 불분명한 불용어이다.
- 불용어는 중복이다. 
	- NameString / Name 같은 의미이고, NameString은 중복된 의미이다.
	- -> 만약 Name이 부동소수가 된다면,  “잘못된 네이밍을 지은것“
	- Customer / CustomerObject 
	- Money / MoneyAmount
	- message / theMessage
	- getActiveAccount() / getActiveAccounts() / getActiveAccountInfo()
	- -> 읽는 사람은 차이를 알지 못한다.

---

## 발음하기 쉬운 이름을 사용하라

- 나쁜예
	- genymdgms (generate date, year, month,day,hour,minute,second 의 약자)
```
class DtaRcrd102 {
    private Date genymdhms;
    private Date modymdhms;
    private final String pszqint = "102";
    /* ... */
};

```
- 좋은 예
```
class Customer {
    private Date generationTimestamp;
    private Date modificationTimestamp;
    private final String recordId = "102";
    /* ... */
}; 
```



## 검색하기 쉬운 이름을 사용해라
- 숫자는 상수로 정의해서 사용하라
- 변수 이름의 길이는 변수의 범위에 비례해서 길어진다.

나쁜 예 

```
for (int j=0; j<34; j++) {
	s += (t[j]*4)/5;
}
```

좋은 예

```
int realDaysPerIdealDay = 4;
const int WORK_DAYS_PER_WEEK = 5;
int sum = 0;
for (int j=0; j< NUMBER_OF_TASKS; j++) {
	int realTaskDays = taskEstimate[j] * realDaysPerIdealDay;
	int realTaskWeeks = (realTaskDays / WORK_DAYYS_PER_WEEK);
	sum+ = realTaskWeeks;
}
```


## 인코딩을 피해라
이름에 인코딩할 정보는 아주많다. 
유형, 범위, 정보까지 인코딩에 넣으면 그만큼 이름을 해독하기 어려울뿐더러, 
발음하기도 어렵고 오타가 생기기도 쉽다.

- 헝가리안 표기법을 피하자 
	- 이름에 데이터 타입을 표기하는 표기법
	- IDE와 컴파일러의 발전으로 헝가리안 표기법은 구 시대적인 사용방법이다.
	- 변수명에 해당 변수의 타입(String, Integer 등등 )을 적지말자
	- PhoneNumber phoneString; (타입이 바뀌어어도 변수명은 바뀌지 않는다..)
- 멤버 변수 접두어
	- 접두어는 구닥다리 코드이다 (ex> m_desc)
	- 접구어가 필요없을 정도로 클래스와 함수는 작아야한다.

```
public class Part {
	private String m_dsc;
	void setName(String name) {
		m_dsc = name;
	}
}

public class Part {
	private String description;
	void setDescription(String description) {
		this.description = description;
	}
}

```

- 인터페이스 클래스와 구현 클래스 
	- 접두어로 I (Interface)  , c(concreate) 를 붙이지 말자..! 
	- 인터페이스 이름과 구현 클래스 이름 중 하나를 인코딩해야한다면, 차라리 구현 클래스 이름을 택하는게 좋다
		- ShapeFactory,ShapeFactoryImp(o)  
		- IShapFactory,CShapeFactory (x)

---

## 자신의 기억력을 자랑하지 마라
누군가 코드를 읽으면서, 변수 이름을 자신이 아는 이름으로 변환하여 생각해야 한다면 그 변수이름은 바람직하지 못한것이다.
일반적으로 사용되는 문제영역/ 해법영역에서 사용하지 않는 이름이기 때문이다.

특정 변수명을 나만 아는 언어로 짓지 말자! 

## 클래스 이름
- 클래스 이름이나 객체 이름은 명사나 명사구가 적합하다. 
	- Customer, WikiPage, Account, AddressParser
- Manager, Processor, Data, Info 등과 같은 단어는 피하고(의미가 모호하기때문에?) , 동사는 사용하지 말자


## 메서드 이름 
- 메서드 이름은 동사나 동사구가 적합하다.
	- postPayment, deletePage, save 등
- 접근자,변경자,조건자는 get,set,is로 javabean 표준에 따라라
- 생성자를 오버로드 할 경우, 정적 팩토리 메서드를 사용하고 해당 생성자를 private으로 선언하라.
	- 좋은 예 : ` Complex fulcrumPoint = Complex.FromRealNumber(23.0); `
	- 나쁜 예 : ` Complex fulcrumPoint = new Complex(23.0); `

## 기발한 이름은 피해라
- 특정 문화에서만 사용되는 언어나 농담보단, 의도를 분명히 표현하는 이름을 사요해라
- ex > HolyHandGrenade , whack , eatMyShort (wtf?)

## 한 개념에 한 단어를 사용해라
- 추상적인 개념하나에 단어 하나를 선택해 통일해라
	- 같은 메서드를 클래스마다  fetch, retrieve, get로 제각각 사용하면, 혼란스럽다.
- 마찬가지로,  controller, manager, driver 를 섞어 써도 의미가 불분명하다.

- 일관성 있는 어휘를 사용하자.

## 말장난하지마라
- 같은 맥락도 아닌데, 일관성을 고려해서 add 라는 단어를 선택한다.
	- add 메서드들과 맥락 ( 매개변수/반환 값의 의미 ) 이 다르다면, append, insert를 사용해라


## 해법 영역에서 가져온 이름을 사용하라
- 개발자라면 잘알고 있는 JobQueue 등과 같은 이름을 지어라
	- 모든 이름을 문제영역 (도메인)에서 가져오는건 좋지 않다.
	- 코드를 보는 사람은 프로그래머이다.
	- 기술개념에는 기술 이름이 가장 적합한 선택이다.

## 문제 영역에서 가져온 이름을 사용해라.
- 적절한 프로그래머 용어가 없다면, 도메인과 관련이 깊은 영어를 사용해라
	- 해당 전문가에게 의미를 물어서 파악할수있다.

## 의미 있는 맥락을 추가해라
- 클래스/함수/ 이름공간에 넣어 맥락을 부여해라
	- ex> firstName , lastName , street, city, **state** , zipcode  (state 변수 따로 쓰일때라면?)
	- -> Address class내의 변수로 선언하면 좋다.
	- -> 모든 방법이 실패했을때, 최종 마지막 수단으로 접두어를 붙여라 ( addrㄴtate )


```
private void printGuessStatistics(char candidate, int count) {
	String number;
	String verb;
	String pluralModifier;
	if ( count ==0 ) {
		number = "no";
		verb = "are";
		pluralModifier = "s";
	} else if (count == 1) {
		number = "1";
		verb = "is";
		pluralModifier = "";
	} else {
		number = Integer.toString(count);
		verb = "are";
		pluralModifier = "s";
	}
	String guessMessage = String.format(
		"There %s %s %s%s", verb, number, candidate, pluralModifier
	);
	print(guessMessage)
}

```

```
pulbic class GuessStatisticsMessage {
	private String number;
	private String verb;
	private String pluralModifier;

	public String make(char candidate, int count) {
		createPluralDependentMessageParts(count);
		return String.format(
			"There %s %s %s%s", verb, number, candidate, pluralModifier
		);
	}
	
	private void createPluralDependentMessageParts(int coint){
		if ( count ==0 ) {
			number = "no";
			verb = "are";
			pluralModifier = "s";
		} else if (count == 1) {
			number = "1";
			verb = "is";
			pluralModifier = "";
		} else {
			thereAreManyLetters(count)
		}
	}

	private void thereAreManyLetters(int count){
		number = Integer.toString(count);
		verb = "are";
		pluralModifier = "s";
	}
	private void thereIsOneLetter(){
			number = "1";
			verb = "is";
			pluralModifier = "";
	}
	private void thereAreNoLetters(){
			number = "no";
			verb = "are";
			pluralModifier = "s";
	}
}


```


## 불필요한 맥락을 업애라
- Gas Station Delux라는 애플리케이션을 만든다고 모든 클래스이름을 GSD로 붙이지말자
- 일반적으로 짧은 이름이 긴 이름보다 좋다 ( 의미가 분명한 경우 !! )
- 이름에 불필요한 맥락을 추가하지 말자.
	-  accountAddress/customerAddress는 Address 클래스의 인스턴스로는 이름이 좋지만, 클래스 이름으로는 별로이다. ( 클래스이름은 Address 좀더 자세히라면 PostalAddress,Mac, Uri 등을 붙여도 좋다)
	

## 결론

좋은 이름을 선택하려면, 설명능력이 뛰어나야하고, 문화적인 배경이 같아야한다.
좋은 이름을 선택하는 것은 기술, 비즈니스, 관리 문제가 아니라 교육의 문제다.

-> 이름을 막 바꿔서 누군가  질책할지도 모르지만, 그렇다고 코드를 개선하려는 노력을 중단해서는 안된다! 






