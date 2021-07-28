# 객체와 자료구조

클래스 내부의 변수를 왜 `private`으로 정의할까? 사람들은 충동이든 변덕이든 변수 타입이나 구현을 맘대로 바꾸고 싶어하기 때문이다.

근데 어째서 수많은 프로그래머가 조회(get), 설정(set) 함수를 당연하게 공개(public)해 비공개 변수를 외부에 노출할까...?

## 자료 추상화

두 클래스 모두 2차원 점을 표현한다. 그런데 한 클래스는 구현을 외부로 노출하고 다른 클래스는 구현을 완전히 숨긴다.

6-1) 구체적인 Point 클래스

```Java
public class Point {
  public double x;
  public double y;
}
```

6-2) 추상적인 Point 클래스

```Java
public class Point {
  double getX();
  double getY();
  void setCartesian(double x, double y);
  double getR();
  double getTheta();
  void setPolar(double r, double theta);
}
```

정말 멋지게도, <strong>6-2)</strong>는 점이 직교좌표계(x, y)를 사용하는지 극좌표계(r, theta)를 사용하는지 알 길이 없다. 둘 다 아닐지도 모른다! 그럼에도 불구하고 인터페이스는 자료구조를 명백하게 표현하고 있다.

사실 <strong>6-2)</strong>는 자료 구조 이상을 표현하고 있다. 클래스 메서드가 접근 방법을 강제한다. 좌표를 읽을 때는 각 값을 개별적으로 읽어야 하고 좌표를 설정할 때는 두 값을 한꺼번에 설정해야 한다.

반면에 목록 <strong>6-1)</strong>은 확실히 직교좌표계를 사용하는걸 알 수 있다. 또한 개별적으로 좌표값을 읽고 설정하게 강제한다. <strong>6-1)</strong>은 구현을 노출한다. 변수를 `private`으로 선언하더라도 각 값마다 조회(get)함수와 설정(set)함수를 제공한다면 구현을 외부로 노출하는 셈이다.

변수 사이에 함수라는 계층을 넣는다고 구현이 저절로 감춰지지 않는다. 구현을 감추려면 <strong>추상화</strong>가 필요하다. 그저 변수를 직접 조작하지 않고 조회 함수와 설정 함수로 변수를 다룬다고 클래스가 되지는 않는다.

> 그보다는 추상 인터페이스를 제공해 사용자가 구현을 모른 채 자료의 핵심을 조작할 수 있어야 진정한 의미의 클래스다.

<br/>
<br/>

<strong>6-3)</strong>과 <strong>6-4)</strong>를 살펴보자.<br/>
<strong>6-3)</strong>은 자동차 연료 상태를 구체적인 숫자 값으로 알려준다. <strong>6-4)</strong>는 자동차 연료 상태를 백분율이라는 추상적인 개념으로 알려준다. <strong>6-3)</strong>은 두 함수가 변수값을 읽어 반환할 뿐이라는 사실이 거의 확실하다. 반면 <strong>6-4)</strong>는 정보가 어디서 오는지 전혀 드러나지 않는다.

6-3) 구체적인 Vehicle 클래스

```Java
public interface Vehicle {
  double getFuelTankCapacityInGallons();
  double getGallonsOfGasoline();
}
```

6-4) 추상적인 Vehicle 클래스

```Java
public interface Vehicle {
  double getPercentFuelRemaining();
}
```

<br/>

### 그래서 뭐가 좋은건가??

<br/>

> 6-1 보다 6-2, 6-3 보다 6-4가 더 좋다.

> 자료를 세세하게 공개하기보다는 <strong>추상적인 개념</strong>으로 표현하는 편이 좋다. 인터페이스나 조회/설정 함수만으로는 추상화가 이뤄지지 않는다.<br/>
> 개발자는 객체가 포함하는 자료를 표현할 가장 좋은 방법을 <strong>심각하게</strong> 고민해야 한다.
> 아무 생각 없이 조회/설정 함수를 추가하는 방법이 가장 나쁘다.

<br/>

## 자료/객체 비대칭

앞서 소개한 두 가지 예제는 객체와 자료 구조 사이에 벌어진 차이를 보여준다.

> 객체는 추상화 뒤로 자료를 숨긴 채 자료를 다루는 함수만 공개한다.

> 자료 구조는 자료를 그대로 공개하며 별다른 함수는 제공하지 않는다.

두 정의는 본질적으로 상반된다.

<strong>6-5)</strong>를 살펴보자. <strong>6-5)</strong>는 <strong>절차적인</strong> 도형 클래스다. Geometry 클래스는 세 가지 도형 클래스를 다룬다. 각 도형 클래스는 간단한 <strong>자료 구조</strong>다. 즉 아무 메서드도 제공하지 않는다.

도형이 동작하는 방식은 Geometry 클래스에서 구현된다.

6-5) 절차적인 도형

```Java
public class Square {
  public Point topLeft;
  public double side;
}

public class Rectangle {
  public Point topLeft;
  public double height;
  public double width;
}

public class Circle {
  public Point center;
  public double radius;
}

public class Geometry {
  public final double PI = 3.141592;

  public double area(Object shape) throws NoSuchShapeException {
    if (shape instanceof Square) {
      Square s = (Square)shape;
      return s.side * s.side;
    }
    else if (shape instanceof Rectangle) {
      Rectangle r= (Rectangle)shape;
      return r.height * r.width;
    }
    else if (shape instanceof Circle) {
      Circle c = (Circle)shape;
      return PI * c.radius * c.radius;
    }

    throw new NoSuchShapeException()
  }
}
```

객체지향 프로그래머가 위 코드를 본다면 코웃음을 칠지도 모른다. 클래스가 절차적이라 비판한다면 맞는 말이다. 하지만 그런 비웃음이 100% 옳다고 말하기는 어렵다. <br/>
만약 Geometry 클래스에 둘레 길이를 구하는 `perimeter()` 함수를 추가한다 가정해보자. 도형 클래스들은 아무 영향도 받지 않는다😀! 도형 클래스에 의존하는 다른 클래스도 마찬가지다😀! 하지만 새 도형을 추가하고 싶다면? Geometry 클래스에 속한 함수를 모두 고쳐야한다😞.

이번에는 <strong>6-6)</strong>을 살펴보자. <strong>객체 지향적인</strong> 도형 클래스다. 여기서 `area()`는 다형(polymorphic)메서드다. Geometry 클래스는 필요 없다. 그러므로 새 도형을 추가해도 기존 함수에 아무런 영향을 미치지 않는다😀. 반면 새 함수를 추가하고 싶다면 도형 클래스를 전부 고쳐야 한다😞(<strike>데자뷰...?</strike>).

```Java
public class Square implements Shape {
  private Point topLeft;
  private double side;

  public double area() {
    return side * side;
  }
}

public class Rectangle implements Shape {
  private Point topLeft;
  private double height;
  private double width;

  public double area() {
    return height * width;
  }
}

public class Circle implements Shape {
  private Point center;
  private double radius;
  public final double PI = 3.131592;

  public double area() {
    return PI * radius * radius;
  }
}
```

앞서 말했듯이, <strong>6-5)</strong>와 <strong>6-6)</strong>은 상호 보완적인 특질이 있다(∵ <strong>6-5</strong>의 도형은 자료 구조로 구현됐고 <strong>6-6)</strong>의 도형은 객체로 구현됐기때문). 사실상 반대다! 그래서 객체와 자료 구조는 근본적으로 양분된다.

> (자료 구조를 사용하는) <strong>절차적인 코드</strong>는 기존 자료 구조를 변경하지 않으면서 새 함수를 추가하기 쉽다. 반면, <strong>객체 지향 코드</strong>는 기존 함수를 변경하지 않으면서 새 클래스를 추가하기 쉽다.

> (객체를 사용하는) <strong>객체 지향 코드</strong>는 기존 자료 구조를 변경하지 않으면서 새 함수를 추가하기 어렵다. 반면, <strong>절차적인 코드</strong>는 기존 함수를 변경하지 않으면서 새 클래스를 추가하기 어렵다.

<br/>
다시 말해, 객체 지향 코드에서 어려운 변경은 절차적인 코드에서 쉬우며, 절차 적인 코드에서 어려운 변경은 객체 지향 코드에서 쉽다!

복잡한 시스템을 짜다 보면 새로운 함수가 아니라 새로운 자료 타입이 필요한 경우가 생긴다. 이때는 클래스와 객체 지향 기법이 가장 적합하다. 반면, 새로운 자료 타입이 아니라 새로운 함수가 필요한 경우도 생긴다. 이때는 절차적인 코드와 자료 구조가 좀 더 적합하다.

분별 있는 프로그래머는 모든 것이 객체라는 생각이 <strong>미신</strong>임을 잘 안다. 때로는 단순한 자료 구조와 절차적인 코드가 가장 적합한 상황도 있다.
<br/>
<br/>

## 디미터 법칙

디미터 법칙은 잘 알려진 휴리스틱으로, 모듈은 자신이 조작하는 객체의 속사정을 몰라야 한다는 법칙이다.

> 휴리스틱: 휴리스틱(heuristics) 또는 발견법(發見法)이란 불충분한 시간이나 정보로 인하여 합리적인 판단을 할 수 없거나, 체계적이면서 합리적인 판단이 굳이 필요하지 않은 상황에서 사람들이 빠르게 사용할 수 있게 보다 용이하게 구성된 간편추론의 방법이다. - 위키피디아

<br/>

앞 절에서 봤듯이, 객체는 자료를 숨기고 함수를 공개한다. 즉, 객체는 조회 함수로 내부 구조를 공개하면 안 된다는 의미다. 그러면 내부 구조를 (숨기지 않고) 노출하는 셈이니까.

디미터 법칙은 "클래스 C의 메서드 f는 다음과 같은 객체의 메서드만 호출해야 한다."고 주장한다.

<ul>
  <li> 클래스 C
  <li> f가 생성한 객체
  <li> f 인수로 넘어온 객체
  <li> C 인스턴스 변수에 저장된 객체
</ul>

하지만 위 객체에서 허용된 메서드가 반환하는 개체의 메서드는 호출하면 안 된다.

다음 코드(아파치 프레임 워크에서 발견)는 디미터 법칙을 어기는 듯이 보인다.

```Java
final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath();
```

위에서 금방 허용된 메서드가 반환하는 객체의 메서드는 호출하면 안 된다고 했던 법칙을 어겼다. `getOptions()` 함수가 반환하는 객체의 `getScratchDir()` 함수를 호출한 후 `getScratchDir()`함수가 반환하는 객체의 `getAbsolutePath()` 함수를 호출하기 한다.

<strong>기차 충돌</strong>

흔히 위와 같은 코드를 <strong>기차 충돌(train wreck)</strong>이라 부른다. 여러 객차가 한 줄로 이어진 기차처럼 보이기 때문이다. 일반적으로 조잡하다 여겨지는 방식이므로 피하는 편이 좋다.

위 코드는 다음과 같이 나누는 편이 좋다.

```Java
Options opts = ctxt.getOptions();
File scratchDir = opts.getScratchDir();
final string ouputDir = scratchDir.getAbsolutePath();
```

위 예제가 디미터 법칙을 위반하는지 여부는 ctxt, Options, ScratchDir이 객체인지 아니면 자료 구조인지에 달렸다. 객체라면 내부 구조를 숨겨야 하므로 확실히 디미터 법칙을 위반한다. 반면, 자료 구조라면 당연히 내부 구조를 노출하므로 디미터 법칙이 적용되지 않는다.

그런데 위 예제는 조회 함수를 사용하는 바람에 혼란을 일으킨다. 코드를 다음과 같이 구현했다면 디미터 법칙을 거론할 필요가 없어진다.

```Java
final String outputDir = ctxt.options.scratchDir.absolutePath;
```

자료 구조는 무조건 함수 없이 공개 변수만 포함하고 객체는 비공개 변수와 공개 함수를 포함한다면, 문제는 훨씬 간단 하겠지만 단순한 자료 구조에도 조회 함수와 설정 함수를 정의하라 요구하는 프레임워크와 표준(ex. bean)이 존재한다. <br/><br/>

## 잡종 구조

이런 혼란으로 인해 때때로 절반은 객체, 절반은 자료 구조인 잡종 구조가 나온다. 잡종 구조는 중요한 기능을 수행하는 함수도 있고 공개 변수나 공개 조회/설정 함수도 들어있다. 공개 조회/설정 함수는 비공개 변수를 그대로 노출한다. 때문에 해당 객체를 사용할때 절차지향 프로그래밍의 자료구조 접근 방식처럼 비공개 변수를 사용하고픈 유혹에 빠지기 쉽다.

> 이런 잡종 구조는 새론운 함수는 물론이고 새로운 자료 구조도 추가하기 어렵다. 양쪽에서 단점만 모아놓은 구조다. 그러므로 잡종 구조는 되도록 피하는 편이 좋다. 프로그래머가 함수나 타입을 보호할지 공개할지 확신하지 못해 (더 나쁘게는 무지해) 어중간하게 내놓은 설계에 불과하다.

<br/><br/>

## 구조체 감추기

다시 돌아와서, 만약 ctxt, options, scratchDir이 진짜 객체라면? 그렇다면 앞서 코드 예제처럼 줄줄이 사탕으로 엮어서는 안 된다. 객체라면 내부 구조를 감춰야 하니까. 그렇다면 코드에서 의도했던 임시 디렉토리의 절대 결로는 어떻게 얻어야 좋을까?

일단 약간 개선된 다음 두 코드를 살펴보자

```Java
ctxt.getAbsolutePathOfScratchDirectoryOption();
```

```Java
ctxt.getScratchDirectoryOptions().getAbsolutePath();
```

첫 번째 방법은 ctxt 객체에 공개해야 하는 메서드가 너무 많아진다. 두 번째 방법은 `getScratchDirectoryOptions()`이 객체가 아니라 자료 구조를 반환한다고 가정할 때의 방법이다. 어느 방법도 썩 내키지 않는다.

ctxt가 객체라면 <strong>뭔가를 하라고</strong> 말해야지 속을 드러내라고 말하면 안 된다. 임시 디렉토리의 절대 경로가 왜 필요할까?? 절대 경로를 얻어 어디에 쓰려고??

다음은 같은 모듈에서 (한참 아래로 내려가서) 가져온 코드다.

```Java
// outputDir이 여기서 필요로 하는 임시 디렉토리의 경로이다.
String outFile = outputDir + "/" + className.replace('.', '/') + ".class";

FileOutputStream fout = new FileOutputStream(outFile);
BufferedOutputStream bos = new BufferedOutputStream(fout);
```

위 코드를 살펴보면, 임시 디렉토리의 절대 경로를 얻으려는 이유가 임시 파일을 생성하기 위한 목적이란 것을 알 수 있다.

그렇다면 ctxt 객체에 임시 파일을 생성하라고 시키면 어떨까?

```Java
BufferedOutputStream bos = ctxt.createScratchFileStream(classFileName);
```

객체에 맡기기에 적당한 임무로 보인다! ctxt는 내부 구조를 드러내지 않으며. 모듈에서 해당 함수는 자신이 몰라야 하는 여러 객체를 탐색할 필요가 없다. 따라서 디미터 법칙을 위반하지 않는다.

<br/><br/>

## 자료 전달 객체

자료 구조체의 전형적인 형태는 공개 변수만 있고 함수가 없는 클래스다. 이런 자료 구조체를 때로는 자료 전달 객체(DTO: Data Transfer Object)라 한다.

DTO

<ul>
  <li> 특히 데이터베이스와 통신하거나 소켓에서 받은 메시지의 구문을 분석할 때 유용
  <li> 흔히 DTO는 데이터베이스에 저장된 가공되지 않은 정보를 애플리케이션 코드에서 사용할 객체로 변환하는 일련의 단계에서 가장 처음으로 사용하는 구조체다.
</ul>

DTO에서 파생된 빈(bean) 구조가 있다. 빈은 비공개(private) 변수를 조회/설정 함수로 조작한다. 책에서는 일종의 사이비 캡슐화라 하면서 일부 OO 순수주의자나 만족시킬 뿐 별다른 이익을 제공하지 않는다고 한다.

<br/><br/>

## 활성 레코드

<ul>
  <li> 활성 레코드는 DTO의 특수한 형태다.
  <li> 공개 변수가 있거나 비공개 변수에 조회/설정 함수가 있는 자료 구조지만, 대게 save나 find와 같은 탐색 함수도 제공한다.
  <li> 활성 레코드는 데이터베이스 테이블이나 다른 소스에서 자료를 직접 변환한 결과다.
</ul>

불행히도 활성 레코드에 비즈니스 규칙 메서드를 추가해 이런 <strong>"자료 구조"</strong>를 <strong>"객체"</strong>로 취급하는 개발자가 흔하다. 하지만 이는 바람직하지 않다. 그렇게 되면 자료 구조도 아니고 객체도 아닌 잡종 구조가 나오기 때문이다.

해결책은 당연하다. 활성 레코드는 자료 구조로 취급한다. 비즈니스 규칙을 담으면서 내부 자료를 숨기는 객체는 따로 생성한다.

<br/><br/>

## 결론

<ul>  
  <li> <strong>객체</strong>는 동작을 공개하고 자료를 숨긴다. 그래서 기존 동작을 변경하지 않으면서 새 객체 타입을 추가하기 쉬운 반면 기존 객체에 새 동작을 추가하기는 어렵다.

  <li> <strong>자료구조</strong>는 별다른 동작 없이 자료를 노출한다. 그래서 기존 자료 구조에 새 동작을 추가하기는 쉬우나, 기존 함수에 새 자료 구조를 추가하기는 어렵다.
</ul>

그래서 시스템을 구현할 때 새로운 자료 타입을 추가하는 유연성이 필요하면 객체가 더 적합하다. 반대의 경우로 새로운 동작을 추가하는 유연성이 필요하면 자료 구조와 절차적인 코드가 더 적합하다.

우수한 소프트웨어 개발자는 편견 없이 이 사실을 직면한 문제에 최적인 해결책을 선택한다.
