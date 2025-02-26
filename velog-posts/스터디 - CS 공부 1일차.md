<h2 id="1-싱글톤-패턴-singleton-pattern">1. 싱글톤 패턴 (Singleton Pattern)</h2>
<p>싱글톤 패턴은 객체를 하나만 생성하여 애플리케이션 전체에서 공유할 수 있도록 하는 디자인 패턴입니다.</p>
<p><strong>특징</strong>
하나의 인스턴스만 생성됨
전역적으로 접근 가능
메모리 절약 및 데이터 공유 용이</p>
<h3 id="구현-방법-lazy-initialization">구현 방법 (Lazy Initialization)</h3>
<pre><code class="language-java">public class Singleton {
    private static Singleton instance;

    private Singleton() {} // 외부에서 인스턴스 생성을 막음

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}</code></pre>
<p>하지만 위 방식은 멀티스레드 환경에서 안전하지 않기 때문에 double-checked locking 또는 static inner class 방식을 추천합니다.</p>
<h3 id="static-inner-class-방식-권장됨">static inner class 방식 (권장됨)</h3>
<pre><code class="language-java">public class Singleton {
    private Singleton() {}

    private static class Holder {
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return Holder.INSTANCE;
    }
}</code></pre>
<p>이 방식은 클래스가 로드될 때 인스턴스가 생성되지 않고, 최초 getInstance() 호출 시 생성되므로 효율적이며 스레드 안전합니다.</p>
<h2 id="2-jvm-java-virtual-machine">2. JVM (Java Virtual Machine)</h2>
<p><strong>JVM(Java Virtual Machine)</strong>은 자바 바이트코드를 실행하는 가상 머신입니다.</p>
<h3 id="jvm의-역할">JVM의 역할</h3>
<p>바이트코드 실행 → 자바 프로그램을 운영체제와 관계없이 실행 가능
메모리 관리 → GC(Garbage Collection)를 통해 자동으로 메모리 관리
플랫폼 독립성 제공 → Write Once, Run Anywhere</p>
<h3 id="jvm의-구성-요소">JVM의 구성 요소</h3>
<p>클래스 로더(Class Loader): .class 파일을 로드하고 메모리에 적재
메모리 영역 (Runtime Data Area): JVM이 관리하는 메모리 공간
실행 엔진(Execution Engine): 바이트코드를 기계어로 변환하여 실행
GC(Garbage Collector): 불필요한 객체를 정리</p>
<h2 id="3-자바-메모리-영역">3. 자바 메모리 영역</h2>
<p>JVM의 메모리 영역은 다음과 같이 구성됩니다.</p>
<h3 id="1-메서드-영역method-area-클래스-영역">1) 메서드 영역(Method Area, 클래스 영역)</h3>
<p>클래스 정보, static 변수, 상수, 메서드 코드가 저장됨
프로그램이 종료될 때까지 유지됨</p>
<h3 id="2-힙heap">2) 힙(Heap)</h3>
<p>객체(instance)들이 저장되는 공간
<strong>GC(Garbage Collector)</strong>가 관리</p>
<h3 id="3-스택stack">3) 스택(Stack)</h3>
<p>메서드 호출 시 지역 변수, 매개변수, 리턴값 저장
메서드 실행이 끝나면 제거됨 (LIFO 방식)</p>
<h3 id="4-pc-레지스터program-counter-register">4) PC 레지스터(Program Counter Register)</h3>
<p>현재 실행 중인 JVM 명령어의 주소를 저장</p>
<h3 id="5-네이티브-메서드-스택native-method-stack">5) 네이티브 메서드 스택(Native Method Stack)</h3>
<p>JNI(Java Native Interface)를 통해 호출된 네이티브 코드(C, C++)의 정보 저장</p>
<h2 id="4-오버로딩overloading과-오버라이딩overriding-차이">4. 오버로딩(Overloading)과 오버라이딩(Overriding) 차이</h2>
<p>구분    오버로딩 (Overloading)    오버라이딩 (Overriding)
의미    같은 이름의 메서드를 여러 개 정의    상속받은 메서드를 재정의
클래스 관계    같은 클래스 내에서 발생    부모 클래스의 메서드를 자식 클래스에서 재정의
매개변수    개수, 타입, 순서를 다르게 정의    동일해야 함
반환 타입    달라도 가능    동일해야 함
접근 제한자    제한 없음    부모보다 좁아질 수 없음
애너테이션    없음    @Override 사용</p>
<p>오버로딩 예제</p>
<pre><code class="language-java">
class MathUtils {
    int add(int a, int b) {
        return a + b;
    }

    double add(double a, double b) { // 매개변수 타입이 다름
        return a + b;
    }
}</code></pre>
<p>오버라이딩 예제</p>
<pre><code class="language-java">
class Parent {
    void showMessage() {
        System.out.println(&quot;부모 클래스&quot;);
    }
}

class Child extends Parent {
    @Override
    void showMessage() { // 부모 메서드 재정의
        System.out.println(&quot;자식 클래스&quot;);
    }
}</code></pre>
<h2 id="5-클래스class-객체object-인스턴스instance-차이">5. 클래스(Class), 객체(Object), 인스턴스(Instance) 차이</h2>
<p>클래스(Class): 객체를 만들기 위한 설계도
객체(Object): 클래스에서 생성된 실제 대상
인스턴스(Instance): 객체가 메모리에 할당된 상태</p>
<p>예제</p>
<pre><code class="language-java">class Car {  // 클래스
    String model;
}

public class Main {
    public static void main(String[] args) {
        Car myCar = new Car(); // 객체 생성 (인스턴스)
    }
}</code></pre>
<p>Car는 클래스
myCar는 객체이자 인스턴스
<strong>즉, &quot;인스턴스는 객체의 특정한 상태를 나타내는 개념&quot;</strong>이라고 볼 수 있습니다.</p>
<h2 id="6-원시-타입primitive-type과-참조-타입reference-type-차이">6. 원시 타입(Primitive Type)과 참조 타입(Reference Type) 차이</h2>
<p>구분    원시 타입 (Primitive Type)    참조 타입 (Reference Type)
데이터 저장 방식    값 자체를 저장    객체의 주소(참조값)를 저장
메모리 위치    스택(Stack)    힙(Heap)
크기    고정적    가변적
기본 값    int → 0, boolean → false    null
비교 방식    값 자체 비교 (==)    주소 비교 (==), 내용 비교 (equals())</p>
<p>원시 타입 예제</p>
<pre><code class="language-java">
int a = 10;
int b = a; // 값이 복사됨
b = 20;
System.out.println(a); // 10 (a의 값은 변하지 않음)</code></pre>
<p>참조 타입 예제</p>
<pre><code class="language-java">class Person {
    String name;
}

Person p1 = new Person();
p1.name = &quot;Alice&quot;;

Person p2 = p1; // 주소가 복사됨
p2.name = &quot;Bob&quot;;</code></pre>
<p>System.out.println(p1.name); // &quot;Bob&quot; (같은 객체를 참조하므로 값이 변경됨)
참조 타입은 같은 객체를 가리키므로 p1과 p2가 같은 데이터를 공유합니다.</p>