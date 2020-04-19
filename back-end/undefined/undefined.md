---
description: 객체지향과 디자인 패턴(최범균 저) 객체지향 파트 정리한 내용입니다.
---

# 객체 지향

## 1. 절차 지향과 객체 지향

절차 지향 : 프로시져로 프로그램을 구성하는 기법을 절차 지향 프로그래밍이라고 부른다.

전자 장비의 꺼짐/켜짐 상태를 저장하기 위해 boolean 타입이고 이름이 isOn이라는 데이터를 사용한다고 가정하다.

이 데이터를 사용하는 프로시저는 모두 isOn을 boolean 타입으로 처리할 것이다. 그런데, 요구 사항이 변경되어서 꺼짐/켜짐 상태뿐만 아니라 대기 상태를 추가해야 한다고 해보자. 이 요구 사항을 반영하기 위해 isOn 데이터의 타입을 boolean 타입에서 열거 타입으로 변경하게 되면, 이 데이터를 사용하는 모든 프로시저도 함께 수정해 주어야 한다.

절차 지향적으로 프로그램을 구성할때 매우 흔하게 밣생하는 문제들이다. 이로 인해 새로운 요구 사항이 생겨서 프로그램의 한 곳을 수정하게 되면, 다른 곳에서 문제가 발생하고, 다시 그 곳을 수정하면 또 다른 곳에서 문제가 발생하는 악순환이 발생하기도 한다.

이는 결국 코드의 수정을 어렵게 만들며, 새로운 기능을 추가하는데 많은 구현 시간\(즉, 개발 비용\)을 투입하게 만든다.

객체 지향 : 데이터 및 데이터와 관련된 프로시저를 객체\(Object\)라고 불리는 단위로 묶는다.

객체는 다른 객체에 기능을 제공하기 위해 프로시저를 사용하는데, 이때 프로시저는 자신이 속한 객체의 데이터에만 접근할 수 있으며, 다른 객체에 속한 데이터에는 접근할 수 없다.

객체 지향적으로 만든 코드에서는 객체의 데이터를 변경하더라도 해당 객체로만 변화가 집중되고 다른 객체에는 영향을 주지 않기 때문에, 요구 사항의 변화가 발생했을 때 절차 지향 방식보다 프로그램을 더 쉽게 변경할 수 있는 장점을 갖는다.

## 2. 객체

객체 지향의 가장 기본은 객체\(Object\)다. 앞ㅎ서 객체는 데이터와 그 데이터를 조작하는 프로시저\(오퍼레이셔느 메서드, 함수\)로 구성된다고 했는데, 이는 객체의 물리적인 특징일 뿐이다. 실제로 객체를 정의할 때 사용되는 것은 객체가 제공해야 할 기능이며, 객체가 내부적으로 어떤 데이터를 갖고 있는 지로는 정의되지 않는다.

보통 객체가 제공하는 기능을 오퍼레이션이라고 부른다.

인터페이스는 객체를 사용하기 위한 일종의 명세나 규칙이라고 생각하면 된다.

객체는 객체가 제공하는 기능으로 정의된다고 했는데, 이는 다시 말하면 객체마다 자신만의 책임\(responsibility\)이 있다는 의미를 갖는다.

객체 지향적으로 프로그래밍을 할 떄, 가장 어려우면서 가장 중요한 것이 바로 객체마다 기능을 할당하는 과정이다.

객체가 얼마나 많은 기능을 제공할 것인가에 대한 확실한 규칙이 하나 존재하는데, 그 규칙은 바로 객체가 갖는 책임의 크기는 작을수록 좋다는 것이다. 객체가 갖는 책임이 작아야 한다는 것은 객체가 제공하는 기능의 개수가 적다는 걸 의미한다.

객체가 갖는 책임이 커질수록 절차 지향적으로 구조가 변질되며, 절차 지향의 가장 큰 단점인 기능 변경의 어려움 문제\(즉, 경직성 문제\)가 발생하게 된다.

단일 책임 원칙은 이름에서 알 수 있듯이 객체는 단 한개의 책임만을 가져야 한다는 원칙이다.

단일 책임 원칙을 따르다 보면 자연스럽게 기능의 세부 내용이 변경될 떄, 변경해야 할 부분이 한 곳으로 집중된다.

41p 예제 참조

이렇게 한 객체가 다른 객체를 생성하거나 다른 객체의 메서드를 호출할 떄, 이를 그 객체에 의존\(dependency\)한다고 표현한다.

45p 예제 참조

* 내가 변경되면 나에게 의존하고 있는 코드에 영향을 준다.
* 나의 요구가 변경되면 내가 의존하고 있는 타입에 영향을 준다.

캡슐화

캡슐화는 객체가 내부적으로 기능을 어떻게 구현하는지를 감추는 것이다.

47p 예제 참조

우여곡절 끝에 만료 여부 확인 코드를 모두 찾아서 수정해 주었다고 하다. 그런데, 시간을 흘러 다시 만료 여부 확인 정책에 변화가 생겼다. 슬프게도 그 사이에 만료 여부를 확인하는 코드를 사용하는 곳이 증가했다.

요구사항의 변화로 인해 데이터의 구조나 쓰임새가 변경되면 이로 인해 데이터를 사용하는 코드들도 연쇄적으로 수정해 주어야 한다.

49p 예제 참조

다시 살펴보니 수정할 곳이 없다. IsExpired\(\) 메서드만 수정했을 뿐, isExpired\(\) 메서드를 사용하는 코드는 변경할 필요가 없는 것이다.

객체 지향을 처음 접하는 사람들은 그 동안 몸에 밴 절차 지향 방식 습관 때문에 무심결에 데이터 중심적인 코드를 만들기 쉽다. 이런 습관을 고치는데 도움이 되는 두 개의 규칙이 있는데 그 규칙은 다음과 같다.

* Tell, Don’t Ask
* 데미테르의 법칙

“Tell, Don’t Ask” 규칙은 간단하다. 데이터를 물어보지 않고, 기능을 실행해 달라고 말하라는 규칙이다. 앞서 회원 만료 여부를 확인하는 코드를 다시 떠올려 보자. 절차 지향 방식은 만 료 일자 데이터를 가져와서, 직접 만료 여부를 확인했다.

50p 코드 참조

데이터를 읽는 것은 데이터를 중ㅅ김으로 코드를 작성하게 만드는 원인이 되며, 따라서 절차지향적인 코드를 유도하게 된다.

기능 실행을 요청하는 방식으로 코드를 작성하다 보면, 자연스럽게 해당 기능을 어떻게 구현했는지 여부가 감춰진다. 즉, 기능 구현이 캡슐화되는 것이다.

데미테르의 법칙은 ‘Tell, Don;t Ask’ 규칙을 따를 수 있도록 만들어 주는 또 다른 규칙이다. 데미테르의 법칙은 다음과 같이 간단한 규칙으로 구성된다.

* 메서드에서 생성한 객체의 메서드만 호출
* 파라미터로 받은 객체의 메서드만 호출
* 필드로 참조하는 객체의 메서드만 호출

51p 예제 참조

데미테르의 법칙에 따르면 파라미터로 전달 받은 객체의 메서드만 호출하도록 되어 있는데, 위 코드의 경우 파라미터로 전달받은 member의 getDate\(\) 메서드를 호출한 뒤에, 다시 getDate\(\)가 리턴한 date 객체의 getTime\(\) 메서드를 호출했기 때문이다.

따라서 데미테르의 법칙을 따르려면, 위 코드를 member 객체 대한 한 번의 메서드 호출로 변경해 주어야 한다. 이는 결국 데이터 중심이 아닌 기능 중심으로 코드를 작성하도록 유도하기 때문에, 기능 구현의 캡슐화를 향상시켜 준다.

52p 예제 참조

객체 지향 설계란 다음의 작업을 반복하는 과정이라고 볼수 있다.

1. 제공해야 할 기능을 찾고 또는 세분화하고, 그 기능을 알맞은 객체에 할당한다.
   1. 기능을 구현하는데 필요한 데이터를 객체에 추가한다. 객체에 데이터를 먼저 추가하고 그 데이터를 이용하는 기능을 넣을 수도 있다.
   2. 기능은 최대한 캡슐화해서 구현한다.
2. 객체 간에 어떻게 메시지를 주고받을 지 결정한다.
3. 과정1과 과정2를 개발하는 동안 지속적으로 반복한다.

객체 설계는 한 번에 완성되지 않고 구현을 진행해 나가면서 점진적으로 완성된다. 이는 최초에 만든 설계가 완벽하지 않으며, 개발이 진행되면서 설계도 함께 변경된다는 것을 의미한다. 따라서 설계를 할 때에는 변경되는 부분을 고려한 유연한 구조를 갖도록 노력해야 한다.

## 3. 다형성과 추상 타입

객체 지향이 주는 장점은 구현 변경의 유연함이다.

상속은 한 타입을 그대로 사용하면서 구현을 추가할 수 있도록 해주는 방법을 제공한다.

다형성은 한 객체가 여러 가지\(poly\) 모습\(morph\)을 갖는다는 것을 의미한다.

구현 상속은 클래스 상속을 통해서 이루어진다. 구현 상속은 보통 상위 클래스에 정의된 기능을 재사용하기 위한 목적으로 사용된다.

추상화는 데이터나 프로세스 등을 의미가 비슷한 개념이나 표현으로 정의하는 과정이다.

추상화된 타입은 오퍼레이션의 시그니터만 정의할 뿐 실제 구현을 제공하지는 못한다.

콘크리트 클래스를 직접 사용해도 문제가 없는데, 왜 추상 타입을 사용하는 것일까?

71p 예제

```text
public class FlowController {
	private boolean useFile;
	public FlowController(boolean useFile) {
		this.useFile = useFile;
	}

	public void process() {
		byte[] data = null;
		if (useFile) {
			FileDataReader fileReader = new FileDataReader();
			data = fileReader.read();
		} else {
			SockerDataReader sockerReader = new SockerDataReader();
			data = sockerReader.read();
		}
		...
} 
```

FlowController 자체는 파일이건 소켓이건 상관없이 데이터를 읽어 오고 이를 암호화해서 특정 파일에 기록하는 책임을 진다.

그런데, flowController의 본연의 책임\(흐름 제어\)과 상관없는 데이터 읽기 구현의 변경 때문에 FlowController도 함께 바뀌는 것이다.

```text
ByteSource source = null;
if (useFile)
	source = new FileDataReader();
else
	source = new SockerDataReader();
byte[] data = source.read();
```

이전 코드보다 약간 단순해졌지만, 여전히 if-else 블록이 남아있다.

ByteSource의 종류가 FlowController가 바뀌지 않도록 하는 방법에는 다음의 두 가지가 존재한다.

* ByteSource 타입의 객체를 생성하는 기능을 별도 객체로 분리한 뒤, 그 객체를 사용해서 ByteSource 생성
* 생성자\(또는 다른 메서드\)를 이용해서 사용할 ByteSource를 전달받기

  public class ByteSourceFactory { public ByteSource create\(\) { if \(useFile\(\)\) return new FileDataReader\(\); else return new SockerDataReader\(\); }

  ```text
    private boolean useFile() {
    	String useFileVal = System.getProperty("useFile");
    	return useFileVal != null && Boolean.valueOf(useFileVal);
    }

    // 싱글톤 패턴 적용
    private static ByteSourceFactory instance = new ByteSourceFactory();
    public static ByteSourceFactory getInstance() {
    	return instance;
    }

    ...
  ```

  }

  public class FlowController { public void process\(\) { ByteSource source = new ByteSourceFactory.getInstance\(\).create\(\); byte\[\] data = source.read\(\);

  ```text
    	...
    }
  ```

  }

이제는 새로운 ByteSource 구현 클래스가 추가되어도 FlowController 클래스의 코드는 영향을 받지 않는다.

즉, 추상화는 공통된 개념을 도출해서 추상 타입을 정의해 주기도 하지만, 많은 책임을 가진 객체로부터 책임을 분리하는 촉매제가 되기도 한다.

사실 추상화를 잘 하려면 다양한 상황에서 코드를 작성하고 이 과정에서 유연한 설계를 만들어 보는 경험을 해봐야 한다.

경험하지 않은 분야라 하더라도 추상화할 수 있는 방법이 하나 있는데, 그것이 바로 변화되는 부분을 추상화하는 것이다. 요구 사항이 바뀔 때 변화되는 부분은 이후에도 변경될 소지가 많다. 이런 부분을 추상 타입으로 교체하면 향후 변경에 유연하게 대처할 수 있는 가능성이 높아진다.

#### 3.4 인터페이스에 대고 프로그래밍하기

실제 구현을 제공하는 콘크리트 클래스를 사용해서 프로그래밍하지 말고, 기능을 정의한 인터페이스를 사용해서 프로그래밍하라는 뜻이다.

추상 타입을 사용하면 기존 코드를 건드리지 않으머ㅕㄴ서 콘크리트 클래스를 교체할 수 있는 유연함을 얻을 수 있는데 이 규칙은 바로 추상화를 통한 유연함을 얻기 위한 규칙이다.

주의할 점은 유연함을 얻는 과정에서 타입\(추상 타입\)이 증가하고 구조도 복잡해지기 때문에 모든 곳에서 인터페이스를 사용해서는 안 된다는 것이다. 이 경우, 불필요하게 프로그램의 복잡도만 증가시킬 수 있다. 인터페이스를 사용해야 할 때는 변화 가능성이 높은 경우에 한해서 사용해야 한다.

따라서 변화 가능성이 높은 콘크리스트 클래스 대신 이를 추상화한 인터페이스를 사용하면 \(다소 구조는 복잡해지지만\) 변경의 유연함이라는 효과를 얻을 수 있지만 변경 가능성이 매우 희박한 클래스에 대해 인터페이스를 만든다면 오히려 프로그램의 구조만 복잡해지고 유연함의 효과는 누릴 수 없는 그런 상황이 발생하게 된다.

## 재사용 : 상속보단 조립

객체 지향의 주요 특징으로 재사용을 말하면서 그 예로 상속을 드는 경우가 있다. 물론, 상속을 사용하면 상위 클래스에 구현된 기능을 그대로 재사용할 수 있기 때문에 상속을 사용하면 재사용을 쉽게 할 수 있는 것은 분명하다.

### 1. 상속을 통한 재사용의 단점

* 상위 클래스 변경의 어려움

  어떤 클래스를 상속받는다는 것은 그 클래스에 의존한다는 뜻이다. 따라서 의존하는 클래스의 코드가 변경되면 영향을 받을 수 있다는 것이다.

  상속 계층을 따라 상위 클래스의 변경이 하위 클래스에 영향을 주기 떄문에, 최악의 경우 상위 클래스의 변화가 모든 하위 클래스에 영향을 줄 수 있다. \(89p 그림 4.2 참조\)

* 클래스의 불필요한 증가

  유사한 기능을 확장하는 과정에서 클래스의 개수가 불필요하게 증가할 수 있다.\(90p 그림 4.4 참조\)

* 상속의 오용

  잘못된 상속으로 인한 잘못된 메서드를 제공할 수 있다.

  ```text
    public class Container extends ArrayList<Luggage> {
    	private int maxSize;
    	private int currentSize;
  
    	public Container(int maxSize) {
    		this.maxSize = maxSize;
    	}
  
    	public void put(Luggage lug) throws NotEnoughSpaceException { ... }
    	public void extract(Luggage lug) { ... }
    	public boolean canContain(Luggage lug) { ... }
    }
  ```

  Container 클래스에 정의된 세 개의 메서드 뿐만 아니라 상위 클래스인 ArrayList 클래스에 등록된 메서드의 목록을 함께 제공된다.

  ArrayList의 add\(\) 메서드를 사용하면 Container의 여분 계산이 정상적으로 동작하지 않기 때문에 에러가 발생할 것이다.

  이건 누구의 잘못일까? Put\(\) 메서드를 사용하지 않고 add\(\) 메서드를 사용한 개발자의 잘못일까? 물론, 잘못은 Container 클래스의 사용법을 제대로 지키지 않은 개발자에 있다. 하지만, 더 큰 잘못은 오용의 여지를 준 Container 클래스 작성자에 있다.

  위와 같은 문제가 발생하는 이유는 Container는 사실 ArrayList가 아니기 때문이다. 상속은 IS-A 관계가 성립할 때에만 사용해야 하는데 “Container는 ArrayList이다.” 라는 IS-A 관계가 성립되지 않는다.

### 2. 조립을 이용한 재사용

객체 조립은 여러 객체를 묶어서 더 복잡한 기능을 제공하는 객체를 만들어내는 것이다.

한 객체가 다른 객체를 조립해서 필드로 갖는다는 것은 다른 객체의 기능을 사용한다는 의미를 내포한다.

앞서 기능이 추가될 때마다 Storage 클래스를 상속받은 하위 클래스가 증가했던 방식과 비교해 봤을 때, 조립을 이용하면 불필요한 클래스 증가를 방지할 수 있다는 것을 알 수 있다.

조립 방식의 또 다른 장점은 런타임에 조립 대상 객체를 교체할 수 있다는 것이다.

상속에 비해 조립을 통한 재사용의 단점은 99p 그림 4.10 에서 보는 것처럼 상대적으로 런타임 구조가 복잡해진다는 것이다. 또 다른 단점은 상속보다 구현이 더 어렵다는데 있다.

* 그렇다면 상속은 언제 사용해야 할까?
  * 상속을 사용할 때에는 재사용이라는 관점이 아닌 기능의 확장이라는 관점에서 상속을 적용해야 한다. \(101p 그림 4.11 참조\)
  * 이처럼 상속은 명확한 IS-A 관계에서 점진적으로 상위 클래스의 기능을 확장해 나갈 때 사용할 수 있다. 단, 최초에는 명확한 IS-A 관계로 보여서 상속을 이용해서 기능을 확장했다고 하더라도, 이후에 클래스의 개수가 불필요하게 증가하는 문제가 발생하거나 상위 클래스의 변경이 어려워지는 등 상위 클래스를 상속받을 때의 단점이 발생한다면, 조립으로 전환하는것을 고려해야 한다.
