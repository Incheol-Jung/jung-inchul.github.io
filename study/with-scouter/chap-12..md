# CHAP 12. 메모리 때문에 발생할 수 있는 문제들

## 자바의 메모리 영역

자바의 메모리 영역\(정확하게 말하면, 실행 시 데이터가 저장되는 여역\(runtime data area\)은 다음과 같이 나뉜다.

* PC\(program counter\) 레지스터 : 스레드별 보유
* JVM 스택 : 스레드별 보유
* 힙\(Heap\) : 대부분의 데이터가 저장되는 일반적인 저장소
* 메서드 영역\(Method area\)
* 런타임 상수 풀\(Runtime Constant Pool\): 메서드 영역에 할당되어 있음
* 네이티브 메서드 스택\(Native Method Stack\)

### PC 레지스터

각 스레드의 JVM 인스트럭션의 주소가 저장되어 있다.

### JVM 스택

스레드가 생성되면서 동시에 생성된다. 이 영역에는 지역 변수와 부분 결과를 저장하며, 메서드 호출 및 리턴과 관련된 정보가 보관된다.

### 힙

모든 클래스의 인스턴스와 배열이 할당된다. 이 영역은 JVM이 시작될 때 생성되며 가비지 컬렉터에 의해서 관리된다. 보통 일반적으로 우리가 개발하는 객체들은 힙 영역에 저장되며, 이 힙 여역은 Eden, Survivor, Tenured, Permanent 영역으로 나뉜다.

추가로 G1 GC는 바둑판 모양으로 메모리 영역이 나뉜다. 이 여러 영역들 중 일부를 New 영역, 나머지를 Old 영역으로 정의해서 사용하는 것이 바로 G1 GC다. 자바는 GC가 발생하면, 다른 영역으로 데이터를 '복사'하는 작업을 수행한다. 아무리 메모리가 빠르다고 하더라도 복사하는 작업을 수행하면 성능저하가 발생할 수밖에 없다. 그래서 G1 GC에서는 New 영역이 꽉 차서 더 이상 메모리를 할당할 공간이 없다면 자기 자신을 Old 영역으로 변환해 보다 빠른 GC를 제공한다.

### 메서드 영역

모든 JVM의 스레드를 공유하며, 각 클래스의 구조 정보를 저장하는 영역이다. 더 자세하게 말하면, 런타임 상수 풀 필드, 메서드 데이터, 메서드와 생성자의 코드, 클래스와 인터페이스 인스턴스의 초기화를 위한 특수 메서드들에 대한 정보들이 들어 있다.

### 런타임 상수 풀

각각의 클래스 및 인터페이스에 대한 contant\_pool 테이블을 실행 시 참고하기 위한 저장소이다.

### 네이티브 메서드 스택

자바 언어 이외의 네이티브 언어를 호출할 경우 타 언어의 스택 정보를 여기에 저장한다.

## OutOfMemoryError는 언제 발생할까?

메모리 릭이 발생하는 상태로 계속 시스템을 놓아두면, 언젠가 OutOfMemoryError가 발생할 수 있다. OutOfMemoryError의 부모 클래스는 java.lang.Error 클래스가 아니고, java.lang.VirtualMachineError라는 클래스ㅏ. 이 클래스는 JVM에 문제가 생겼거나 작업을 계속해서 진행할 리소스가 부족할 때 발생한다고 API에 나와 있다. 이 클래스의 자식 클래스로는 OutOfMemoryError를 비롯하여 InternalError, TrackOverflowError, UnknownError가 있다.

#### 그렇다면 언제 OutOfMemoryError가 발생할까?

* 가비지 컬렉터가 새로운 객체를 생성할 공간을 더 이상 만들어주지 못하고, 더이상 힙 영역의 메모리가 증가될 수 없을 때
* 네이티브 라이브러리 코드에서 스왑 영역이 부족하여, 더 이상 네이티브 할당\(allocation\)을 할 수 없을 때\(자주 발생하지는 않는다\)

두 번째 경우는 JNI와 같은 네이티브 라이브러리를 호출하는 소스를 사용할 경우에 발생하거나 순수 자바코드를 사용할 때 발새하므로 발생 빈도가 적으므로 첫 번째 경우에 대해서만 상세히 알아보자

OutOhMemoryError가 발생할 때 찍히는 자바의 시스템 로그는 잘 살펴봐야만 한다. 왜냐하면 OutOfMemoryError가 발생하는 원인은 다음과 같이 다양하기 때문이다.

* Exception in thread "main" : java.lang.OutOfMemotyError: java heap sapce
* Exception in thread "main" : java.lang.OutOfMemotyError: Metaspace
* Exception in thread "main" : java.lang.OutOfMemotyError: Requested array size exceeds VM limit
* Exception in thread "main" : java.lang.OutOfMemotyError: request &lt;size&gt; bytes for &lt;reason&gt;. Out of swap space?
* Exception in thread "main" : java.lang.OutOfMemotyError: &lt;reason&gt; &lt;stacktrace&gt; \(Native method\)

다음 절에서 이 내용을 좀 더 자세히 들여다보자

## OutOfMemoryError 메시지의 의미

### Exception in thread "main" : java.lang.OutOfMemotyError: java heap sapce

자바의 힙 영역에 더 이상 객체를 생성하기 어려울 때 이 메시지를 출력한다. 이 메시지만으로 메모리 릭이라고 판단하기는 어렵지만, 다음과 같은 여러 가지 상황에서 이 메시지가 출력될 수 있다.

* 메모리 크기를 너무 적게 잡아 놓거나, 아예 메모리 크기를 지정하지 않은 경우 : 자바 실행 옵션 중 -Xms는 JVM 메모리의 최소 크기를, -Xmx는 JVM의 최대 메모리 크기를 지정한다. 예를 들어, 최소 최대 크기를 512MB로 지정하고 싶으면 -Xms512m -Xmx512m으로 지정하면 된다.
* 오래된 객체들이 계속 참조되고 있어서 GC가 되지 않는 경우 : static을 잘못 사용하는 경우나 애플리케이션이 의도치 않게 객체를 지속해서 참조할 경우 문제가 발생할 수 있다. 이것이 바로 메모리 릭이 발생하는 경우다.
* finalize 메서드를 개발자가 개발한 클래스에 구현해 놓은 경우 : GC가 발생하기 전에 GC가 되어야 하는 객체들은 큐에 쌓이도록 되어 있다. 그래서 오라클의 JVM에서는 자바의 GC를 처리하기 위한 데몬 스레드가 존재하며, 이 스레드에서 쓰레기 객체들을 처리한다. 이 스레드가 청ㅇ소할 객체들을 처리하기 전에 finalize 큐에 너무 많은 객체가 보관되어 있고, 처리도 불가능할 경우 문제가 발생할 수 있다. 이런 문제가 발생하는 경우가 많지는 않지만, 간혹 개발자가 해당 메서드를 구현하여 별도의 작업을 할 때 발생하기도 한다.
* 스레드의 우선순위를 너무 높일 경우 : 개발된 프로그램의 스레드 우선순위를 너무 높게 지정해 놓으면, GC를 처리하는 속도보다 우선순위가 높은 스레드를 메모리에 생성하는 속도가 더 빨라 문제가 발생할 수 있다.
* 큰 덩어리의 객체가 여러 개 있을 경우 : 예를 들어, 한번 호출되면 100MB\(Mega bytes\)의 메모리를 점유하는 화면이 있다고 생각해 보자. 그 서버를 256MB로 설정했을 때, 동시에 세 개의 요청을 그 화면에서 수행할 경우는 어떻게 될까? 그럴 때에도 마찬가지로 힙 영역에 OutOfMemoryError가 발생한다.

### Exception in thread "main" : java.lang.OutOfMemotyError: Metaspace

Java 7 이전의 JVM에서는 Permgen Space라는 메시지가 나왔지만, Java 8 이후에는 Permanent 영역이 없어졌기 때문에 Metaspace 오류가 대신 발생한다. 보통 너무 많은 클래스가 해당 자바 프로세스에 로딩될 경우 이 메시지가 나타날 수 있다. 이 경우 현재 사용하고 있는 메타 영역의 크기보다 크게 지정하면 되며, 관련 옵션은 -XX:MaxMetaspaceSize=128m를 사용하면 된다.

### Exception in thread "main" : java.lang.OutOfMemotyError: Requested array size exceeds VM limit

배열의 크기가 힙 영역의 크기보다 더 크게 지정되었을 때 발생한다. 자주 발생하지는 않겠지만, 배열의 크기를 고정된 크기로 지정하지 않고 계산된 변수로 지정할 경우 이 오류가 발생할 수도 있을 것이다. 그러므로 이 오류가 발생한 경우에는 해당 프로그램의 소스 코드에 이상이 없는지 확인해야 한다.

### Exception in thread "main" : java.lang.OutOfMemotyError: request &lt;size&gt; bytes for &lt;reason&gt;. Out of swap space?

이 메시지는 네이티브\(Native\) 힙 영역이 부족할 때 발생하는 메시지다. 결론은 OS의 메모리가 부족한 상황이 되었을 때, 다시 말해서 OS의 Swap 영역까지도 부족하면 이 메시지가 발생한다. 즉,

* 개발된 자바 애플리케이션에서 호출하는 네이티브 메서드에서 메모리를 반환하지 않는 경우
* 다른 애플리케이션에서 메모리를 반환하지 않는 경우

이와 같은 오류가 발생할 수 있다. 이 메시지가 나타나면, 치명 오류 처리 매커니즘이 JVM에서 작동하여 치명 에러 파일을 발생시킨다. 그 파일에는 에러가 발생할 당시의 스레드, 프로세스, 시스템에 대한 상세한 정보가 들어 있다.

### Exception in thread "main" : java.lang.OutOfMemotyError: &lt;reason&gt; &lt;stacktrace&gt; \(Native method\)

이것도 바로 앞의 경우와 마찬가지로, 네이티브 힙 영역에 메모리를 할당할 때 발생하는 메시지다. 앞에서 설명한 메시지와 다른 점은, 앞의 경우는 JVM 코드에서 발견될 때 발생하고, 이 경우는 메모리 할당 오류가 JNI나 네이티브 코드에서 발생한다는 것이다.

## 메모리 릭의 세 종류

메모리 릭은 어디선가 객체가 GC되지 않아 메모리가 부족해지는 현상을 말한다. 이는 코드에 문제가 있을 수도 있고, 클래스 로더 문제일 수도 있다. 메모리 릭은 다음과 같이 세가지로 분류될 수 있다.

* 수평적 메모리 릭
* 수직적 메모리 릭
* 대각선 형태의 메모리 릭

### 수평적 메모리 릭

하나의 객체에서 매우 많은 객체를 참조하는 경우가 여기에 속한다. 예를 들어 ArrayList와 같은 목록 형태나 배열에서 객체들을 계속 참조하고 놓아주지 않을 때 이러한 형태를 보인다.

### 수직적 메모리 릭

각 객체들이 링크로 연결되었을 경우가 여기에 속한다. 가장 대ㅛ적인 것이 LinkedList를 사용할 경우다.

### 대각선 형태의 메모리 릭

일반적으로 객체들이 복합적으로 메모리를 점유하고 있는 경우가 여기에 속한다.

## OutOfMemoryError 이외의 메모리 문제는 없을까?

OutOfMemoryError 이외의 가장 큰 메모리 문제를 뽑으라고 하면, 크래시\(crash\)가 발생하는 경우다. 크래시 발생 원인이 반드시 메모리 때문인 것은 아니다. 하지만 메모리 문제가 있을 때에도 이 문제가 발생할 수 있다. 이 경우에는 서버의 자바 프로세스가 사라지게 되는데, 보통 네이티브 힙에 메모리 할당이 실패하면 이러한 경우가 발생한다. 예를 들어, 가용한 메모리가 부족하여, malloc이라는 시스템 호출이 널\(null\)을 리턴할 경우가 여기에 속한다. 시스템은 크래스기 발생하면 "hs\_err\_pid"로 시작하는 덤프 파일을 생성하는데, 이 파일이 가끔 도움이 될 수도 있다.

크래시 이외에 다른 메모리 문제라고 하면, 너무 잦은 GC라고 할 수 있다. 정확하게 말하면 너무 잦은 Full GC다. GC에는 Minor GC와 Full\(Major\) GC 두 가지가 있다. 일반적으로 Minor GC가 자주 발생하는 것은 성능에는 큰 영향을 주지 않으며, Full GC가 많이 발생하면 성능에 많은 영향을 미친다.

### 너무 많은 GC를 발생시키면 어떻게 해야 할까?

보통은 GC 튜닝을 생각하지만 그건 제일 마지막이다. 가장 먼저 해야 하는 작업은 GC가 많이 발생하지 않도록 하는 것이다. GC를 발생시키지 않으려면 다음과 같은 규칙들을 따르면 된다.

* 임시 메모리의 사용을 최소화
* 객체의 재사용
* XML 처리 시 메모리를 많이 점유하는 DOM보다 SAX를 사용
* 너무 많은 데이터를 한 번에 보여주는 비즈니스 로직 제거
* 기타 프로파일링을 통하여 임시 메모리를 많이 생성하는 부분 제거

이러한 메모리 튜닝 작업을 마친 이후에도 지속해서 GC가 자주 발생한다면, 그때에는 GC 튜닝을 하는 단계로 넘어간다. GC 빈도수를 확인하는 방법은 크게 다음과 같이 나눌 수 있다.

* 모니터링 도구 : 상용 혹은 무료 모니터링 도구 사용
* verbosegc 옵션 사용 : 자바 애플리케이션을 시작할 때 옵션을 지정하여 사용
* jstat 사용 : 자바ㅏ 프로세스 id\(pid\)만 알면, 각 영역별로 메모리를 얼마나 사용하는지 확인할 수 있는 jstat 사용

저자는 jstat을 권장한다. 그 이유는 jstat은 따로 프로그램을 서리할 필요도 없고, 애플리케이션을 시작할 때 옵션을 줄 필요도 없고, 성능에도 거의 지장이 없기 때문이다.
