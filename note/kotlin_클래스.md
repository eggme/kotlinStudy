## Kotlin의 클래스와 상속
#### 클래스
- class 키워드로 선언함
  - 클래스 이름
  - 클래스 헤더 (형식, 매개변수, 기본 생성자 등)
  - 클래스 바디 (중괄호 { })
```kotlin
class Invoce(data: Int){

}
```
- 헤더와 바디는 옵션이고, 바디가 없으면 {} 도 생략 가능
```kotlin
class Empty
```
##### 기본 생성자
- 클래스 별로 1개만 가질 수 있음
- 클래스 헤더의 일부
- 클래스 이름 뒤에 작성
```kotlin
class Person constructor(firstName: String){

}
```
- 어노테이션이나 접근지정자가 없을 때는, 기본생성자의 constructor 키워드를 생략 가능
```kotlin
class Person(firstName: String) {

}
```
- 기본생성자는 코드를 가질 수 없음
  - 초기화는 초기화(init) 블록 안에서 작성해야 함
  - 초기화 블록은 init 키워드로 작성
- 기본생성자의 파라미터는 __init 블록__ 안에서 사용 가능함
```kotlin
class Customer(name: String){
  init{
      logger.info("Customer initialized with value ${name}")
  }
}
```
- 기본생성자의 파라미터는 프로퍼티 초기화 선언에서도 사용 가능
```kotlin
class Customer(name: String) {
    val customerKey = name.toUpperCase()
}
```
- 프로퍼티 선언 및 초기화는 기본생성자에서 간결한 구문으로 사용 가능
```kotlin
class Person(val firstName: String, val lastName: String) {

}
```
- 기본생성자에 어노테이션 접근지정자 등이 있는 경우 consturctor 키워드가 필요함
```kotlin
class Customer public @Inject constructor(name: String) {}
```
##### 보조생성자
- 클래스 별로 여러 개를 가질 수 있음
- constructor 키워드로 선언
```kotlin
class Person{
    constructor(parent: Person){
        parent.children.add(this)
    }
}
```
- 클래스가 기본생성자를 가지고 있다면, 각각의 보조생성자들은 기본생성자를 직접 or 간접적으로 위임해 주어야 함
- this 키워드를 이용
  - 직접적 : 기본생성자에게 위임
  - 간접적: 다른 보조생성자에 위임
```kotlin
class Person(val name: String){
      constructor(name: String, parent: Person) : this(name){
          // name은 기본생성자에게 위임
      }
      
      constructor() : this("홍길동", Person()){
          // 간접적으로 다른 보조생성자에게 위임
      }
}
```
- 생성된(generated) 기본생성자 (default 생성자)
  - 클래스에 기본생성자 or 보조생성자를 선언하지 않으면, 생성된 기본생성자가 만들어짐
  - generated primary constructor
    - 매개변수가 없음
    - 가시성이 public임
  - 만약 생성된 기본생성자의 가시성이 public이 아니어야 한다면, 다른 가시성을 가진 빈 기본생성자를 선언해야 함
```kotlin
class DontCreateMe private constructor() {

}
```
##### 인스턴스 생성
- 코틀린은 new 키워드가 없음
- 객체를 생성하려면 생성자를 일반 함수처럼 호출하면 됨
```kotlin
val invoice = Invoice()
val customer = Customer("Joe Smith")
```
##### 클래스 맴버
- Constructor and initializer blocks
- Functions
- Properties
- Nested and Inner Class
- Object Declarations

### 상속
- 코틀린의 최상위 클래스는 Any임
- 클래스에 상위 타입을 선언하지 않으면 Any가 상속됨
```kotlin
class Example1 // 암시적으로 Any 상속
class Example2 : Any() // 명시적인 Any 상속
```
- Any는 Java.lang.Object와는 다른 클래스임
  - equals(). hashCode(), toString() 만 있음
```kotlin
package kotlin
public open class Any {
    public open operator fun eqauls(other: Any?): Boolean
    public open fun hashCode(): Int
    public open fun toString(): String
}
```
- 명시적으로 상위 타입을 선언하려면, 클래스 헤더의 콜론(:) 뒤에 상위 타입을 선언하면 됨
```kotlin
open class Base(p: Int)

class Derived(p: Int): Base(p)
```
- 파생클래스에 기본생성자가 __있으면,__ 파생클래스의 기본생성자에서 상위타입의 생성자를 호출해서 초기화할 수 있음
- 파생클래스에 기본생성자가 __없으면,__ 각각의 보조생성자에서 상위타입을 super 키워드를 이용해서 초기화 해주어야 함
- or 다른 생성자에게 상위타입을 초기화할 수 있게 위임을 해주어야 함
```kotlin
class MyView : View{
    consturctor() : super(1)
    constructor(ctx: Int) : this()
    constructor(ctx: Int, attrs: Int) : superr(ctx, attrs)
}
```
- open 어노테이션은 Java의 final과 반대임
- 코틀린에서 기본적으로 모든 클래스는 final임
- open class는 다른 클래스가 상속할 수 있음
##### 메소드 오버라이딩
- __오버라이딩 될 메소드__
  - open 어노테이션이 요구됨
- __오버라이딩 된 메소드__
  - override 어노테이션이 요구됨
```kotlin
open class Base {
    open fun v() {}
    fun nv() {}
}

class Derived: Base(){
    override fun v() {}
}
```
##### 프로퍼티 오버라이딩
- 메소드 오버라이딩과 유사한 방식으로 오버라이딩 가능
```kotlin
open class Foo {
    open val x: Int get { }
}

class Bar1 : Foo() {
    override val x: Int = 
}
```
##### 오버라이딩 규칙
- 같은 멤버에 대한 중복된 구현을 상속받은 경우, 상속받은 클래스는 해당 멤버를 오버라이딩하고 자체 구현을 제공해야 함
- super + <클래스명>을 통해서 상위 클래스를 호출할 수 있음
```kotlin
open class A {
    open fun f() { print("A")
    fun a() { print("a")
}
```
```kotlin
interface B {
    fun f() { print("B") }
    fun b() { print("b") }
}
```
```kotlin
class C : A(), B {
    override fun f(){
        super<A>.f() // call to A.f()
        super<B>.f() // call to B.f()
    }
}
```
#### 추상클래스
- abstract 멤버는 구현이 없음
- abstract 클래스나 멤버는 open이 필요 없음 (기본적으로 open 상태)
```kotlin
abstract class AbsClass {
    abstract fun f()
}
class MyClass: AbsClass() {
    override fun f() {  }
}
```

## Properties and Fields
- 프로퍼티 선언
  - 코틀린 클래스는 프로퍼티를 가질 수 있음
  - var (mutable) / val (read-only)
```kotlin
class Address {
    var name: String = "Kotlin"
    val city: String = "Seoul"
}
```
  - 프로퍼티의 사용은 자바의 필드를 사용하듯이 하면 됨
```kotlin
fun copyAddress(address: Address): Address {
    val result = Address()
    result.name = address.name
    
    return result
}
```
- 프로퍼티 문법
```kotlin
var <propertyName>[:<PropertyType>] [=<property_initializer>]
      [<getter>]
      [<setter>]
```
- 옵션 (생략 가능)
  - PropertyType
    - property_initializer에서 타입을 추론 가능한 경우 생략가능
  - property_initializer
  - getter
  - setter
- 프로퍼티는 OOP, 객체지향 언어에서 제공하는 기능 중 하나, (Getter, Setter를 언어레벨에서 지원하는 기능이라고 생각하면 될듯)
- 마치 필드를 사용하는 것 처럼 쉽게 사용할 수 있지만 사실은 함수를 사용하는 것, 정보은닉 구현
- 장점, 쉽게 작성할 수 있고, 쉽지만 호출의 결과가 함수이기 때문에, vaildation 등을 처리할 수 있음 

##### var(mutable) 프로퍼티
```kotlin
class Address {
    // default getter와 setter
    // 타입은 Int
    val initialized = 1
    
    // error: 오류 발생
    // default getter와 setter를 사용한 경우
    // 명시적인 초기화가 필요함
    var allByDefault: Int?
}
```
##### val(read-only) 프로퍼티
- var 대신 val 키워드 사용
- setter가 없음
```kotlin
class Address {
    // default getter
    // 타입은 Int
    val initialized = 1
    
    // error: 오류 발생
    // default getter
    // 명시적인 초기화가 필요함
    var allByDefault: Int?
}
```

##### Custom accessors (getter, setter)
- custom accessor는 프로퍼티 선언 내부에, 일반 함수 처럼 선언할 수 있음
- getter
```kotlin
val isEmpty: Boolean
  get() = this.size == 0
```
- setter
  - 관습적으로 setter의 파라미터 이름은 value임 (변경가능)
```kotlin
var stringRepresentation: String
  get() = this.toString()
  set(value){
      setDataFromString(value)
  }
```
- 타입 생략
  - 코틀린 1.1 부터는 getter를 통해 타입을 추론할 수 있는 경우, 프로퍼티의 타입을 생략할 수 있음
```kotlin
val isEmpty: Boolean
  get() = this.size == 0
```
```kotlin
val isEmpty
  get() = this.size == 0
```
- accessor에 가시성 변경이 필요하거나, accessor에 어노테이션이 필요한 경우, 기본 accessor의 수정 없이 body 없는 accessor를 통해 정의 가능
```kotlin
var setterVisibility: String = "abc"
    private set
var setterWithAnnotation: Any? = null
    @Inject set // annotate the setter with Inject
```
- Body를 작성해 주어도 됨
```kotlin
var setterVisibility: String = "abc"
    private set(value){
        field = value
    }
```
##### Backing Fields
- 코틀린 클래스는 field를 가질 수 없음
- 'field'라는 식별자를 통해 접근할 수 있는, automatic backing field를 제공함
- field는 프로퍼티의 accessor에서만 사용가능 함
```kotlin
// the initializer value is written directly
// to the backing field
var counter = 0
    set(value) {
        if(value >= 0) field = value
    }
```
- 아래의 경우는 backing field를 생성하지 않음
```kotlin
val isEmpty: Boolean
    get() = this.size == 0
```
##### Backing Properties
- "implicit backing field" 방식이 맞지 않는 경우에는 "backing property"를 이용할 수도 있음
```kotlin
private var _table: Map<String, Int>? = null
public val table: Map<String, Int>
    get() {
        if(_table == null){
            _table = HashMap()
        }
        return _table ?: throw AssertionError("null")
    }
```
##### Compile-Time Constants
- const modifier를 이용하면 컴파일 타임 상수를 만들 수 있음
  - 이런 프로퍼티는 어노테이션에서도 사용 가능함
- 조건
  - Top-level 이거나
  - object의 멤버이거나
  - String나 프리미티브 타입으로 초기화된 경우
```kotlin
const val SUBSYSTEM_DEPRECATED String: "Thie subsystem is deprecated"

@Deprecated(SUBSYSTEM_DEPRECATED)
fun foo() {}
```
##### Late-Initialized Properties
- 일반적으로 프로퍼티는 non-null 타입으로 선언됨
- 간혹 non-null 타입 프로퍼티를 사용하고 싶지만, 생성자에서 초기화를 해줄 수 없는 경우가 있음
  - Dependency Injection
  - Butter knife (안드로이드에서 뷰 바인딩을 쉽게 해주는 api 인듯?)
  - Unit test의 setUp 메소드
```kotlin
public class MyTest{
    lateinit var subject: TestSubject
    
    @SetUp fun setup(){
        subject= TestSubject()
    }
    @Test fun test(){
        subject.method()
    }
}
```
- 객체가 constructor에서는 할당 되지 못하지만 여전히 non-null 타입으로 사용하고 싶은 경우
- Lateinit modifier를 사용 가능
- 조건
  - 클래스의 바디에서 선언된 프로퍼티만 가능
  - 기본생성자에서 선언된 프로퍼티는 안됨
  - var 프로퍼티만 가능
  - custom accessor이 없어야 함
  - non-null 타입이어야 함
  - 프리미티브 타입이면 안됨
- lateinit 프로퍼티가 초기화되기 전에 접근하면, 오류가 발생됨
- kotlin.UninitializedPropertyAccessException: lateinit property tet has not been initialized

## Data, Nested Classes 
- Data 클래스
  - 용도 : 데이터는 보유하지만 아무것도 하지 않는 클래스
  - 코틀린에서는 data class 제공
```kotlin
data class User(val name: String, val age: Int)
```
- __기본 생성자에서 선언된 속성을 통해,__ 아래의 기능들을 컴파일러가 자동으로 생성해 줌
   - eqauls()
   - hashCode()
   - copy()
   - toString() of the form "User(name=John, age=42)",
   - componentN() functions
- 명시적으로 정의해주는 경우에는, 컴파일러가 자동으로 생성해 주지 않음
- 의미 있는 Data 클래스의 조건
  - 기본생성자에 1개 이상의 파라미터
  - 기본생성자에 파라미터가 var, val로 선언
  - Data클래스는 abstract, open, sealed, inner가 안됨
- 1.1 이후에 바뀐 점
  - Data 클래스 interface 구현 가능
  - Sealed class 상속가능
#### Data Classes
- 기본 값
  - Jvm에서 파라미터가 없는 생성자가 필요한 경우, 모든 프로퍼티에 기본 값을 설정해주면 됨
```kotlin
data class User(val name: String = "", val age: Int = 0)
```
```kotlin
val exam_0 = User()
val exam_1 = User("Kotlin")
val exam_2 = User("Kotlin", 113)
val exam_3 = User(age=113)
val exam_4 = User(name="Kotlin", age=113)
```
- 복사
  - 객체의 기존 값들은 유지하고, 일부 값만 고쳐서 새로운 객체를 만들고 싶은 경우
  - Data 클래스에 컴파일러가 copy를 만들어주기 때문에 바로 copy를 호출해서 사용하면 됨
```kotlin
fun copy(name: String = this.name, age:Int = this.age) = User(name, age)
```
```kotlin
val jack = User(name = "Jack", age = 1)
val olderJack = jack.copy(age = 2)
```
#### Destucturing Declarations
- data 클래스는 Destructuring Declarations을 사용 가능함
- 컴파일러가 componentN 함수를 자동으로 만들어주기 때문
- ComponentN 함수가 Destucturing Declarations에서 사용됨, 밑의 코드의 (name, age)가 적용된 예시
```kotlin
val jane = User("Jane", 35)
val (name, age) = jane

println("$name $age year of age")
```
#### Standard Data Classes
- 스텐다드 라이브러리에서 제공하는 data 클래스도 있음
  - Pair
  - Triple
- 물론 가독성을 위해서는 프로퍼티에 의미있는 이름을 제공하는 클래스가 최고임

#### Nested Classes
- 클래스는 다른 클래스에 중첩될 수 있음
```kotlin
class Outer {
    private val bar: Int = 1
    
    class Nested {
        fun foo() = 2
    }
}
val demo = Outer.Nested().foo() // == 2
```
- 내부 클래스는 아니고 중첩클래스임, Nested 클래스는 Outer의 bar에 접근할 수 없음

#### Inner Class
- 클래스에 inner를 표기하면 바깥쪽 클래스 멤버에 접근할 수 있음
```kotlin
class Outer {
    private val bar: Int = 1
    
    inner class Inner {
        fun foo() = bar
    }
}
val demo = Outer.Inner().foo() // == 1
```
#### Anonymous inner Classes
- 객체 표현식(object expression)을 이용해서 익명 내부 클래스의 인스턴스를 생성할 수 있음
```kotlin
mSearchEditText.setOnClickListener(object: View.OnClickListener {
    override fun onClick(v: View?){
        // ...
    }
})
```
- Functional java interface인 경우에는 접두어에 인터페이스 이름을 사용하여 람다식으로도 표현할 수 있음
```kotlin
mSearchEditText.setOnClickListener(View.OnClickListener {
    v -> //...
})
```

## Object Expressions and Declarations
- object 용도
  - 어떤 class에서 조금 변경된 객체를 생성 할 때
  - 새로운 subclass의 명시적인 선언 없이 객체 생성
- Object Expressions
  - Java 익명 객체
- Object Declarations
  - 싱글턴
- Companion Object
  - 싱글턴 + Class method (static)
    - 코틀린은 스태틱 메소드, 스태틱 멤버가 없다
    - 패키지 레벨의 컴페니언 오브젝트를 사용하라고 권장됨
#### 객체 표현식
- Java에서는 익명 내부 클래스를 사용해서 처리했음
- Kotlin 에서는 object expressions을 이용
```Java
btn.setOnClickListener(new OnClickListener(){
    @Override
    public void onClick(View v){
    
    }
});
```
##### 객체 표현식 문법
  - 어떤 클래스를 상속 받은 익명 객체를 생성
```kotlin
window.addMouseListener(object: MouseAdapter() {
    override fun mouseClicked(e: MouseEvent) {}
    override fun mouseEntered(e: MouseEvent) {}
})
```
##### 객체 표현식 상속
- 슈퍼타입의 생성자가 있는 경우, 반드시 값을 전달해주어야 함
- 슈퍼타입이 여러 개인경우, ':'콜론 뒤에 ',' 콤마로 구분하여 명시해주면 됨
```kotlin
open class A(x: Int){
    public open val y: Int = x
}

interface B {...}

val ab: A = object: A(1), B {
    override val y = 15
}
```
##### 객체 표현식 상속 없는 경우
- 특별히 상속받을 superType이 없는 경우, 간단하게 작성 가능
```kotlin
val adHoc = object {
    var x : Int = 0
    var y : Int = 0
}
println(adHoc.x + adHoc.y)
```
##### 객체 표현식 제약 사항
- 익명 객체가 local이나 private으로 선언될 때만 type으로 사용될 수 있음
- 익명 객체가 public funcation이나 public property에서 리턴되는 경우, 익명객체의 슈퍼타입으로 동작됨, 이런 경우 익명 객체에 추가된 멤버에 접근이 불가능함
```kotlin
class C {
    private fun foo() = object {val x: String = "x"}
    fun publicFoo() = object {val x: String = "x"}
    
    fun bar() {
        var x1 = foo().x            // Works
        var x2 = publicFoo().x      // ERROR
    }
}
```
##### 객체 표현식 특징
- 익명 객체의 코드는 enclosiong scope의 변수를 접근할 수 있음
- Java와는 다르게 final variables 제약 조건이 없음
```kotlin
fun countClicks(window: JComponent){
    var clickCount = 0
    var enterCount = 0
    window.addMouseListener(object: MouseAdapter() {
        override fun mouseClicked(e: MouseEvent){
            clickCount++
        }
        override fun mouseEntered(e: MouseEvent){
            enterount++
        }
    })
}
```
##### 객체 선언 용도
- 매우 유용한 Singleton 패턴을 지원
- Kotlin에서는 object declarations을 이용해서 만들 수 있음
```kotlin
object DataProviderManager {
    fun registerDataProvider(provider: DataProvider){
        // ...
    }
    
    val allDataProvider: Collection<DataProvider>
          get() //...
}
```
##### 객체 선언 문법
- object 키워드 뒤에 항상 이름이 있어야 함
- object declaration은 object expression이 아님
- 그래서 할당 구문의 우측에 사용될 수가 없음
```koltin
object DataProviderManager{
    fun registerDataProvider(provider: DataProvider) {}
    val allDataProviders: Collection<DataProvider>
}
```
- object declaration의 객체를 참조하려면, 해당 이름으로 직접 접근하면 됨
```kotlin
DataProviderManager.registerDataProvider(...)
```
- 슈퍼타입을 가질 수 있음(상속 가능)
```koltin
object DefaultListener: MouseAdapter() {
    override fun mouseClicked(e: MouseEvent) { }
    override fun mouseEntered(e: MouseEvent) { }
}
```
##### 동반자 객체
- 클래스 내부의 object declaration은 companion 키워드를 붙일 수 있음
```kotlin
class MyClass {
    companion object Factory {
        fun create(): MyClass = MyClass()
    }
}
```
- companion object의 멤버는 클래스 이름을 통해서 호출 할 수 있음
```kotlin
val instance = MyClass.create()
```
- Companion object의 이름은 생략 될 수 있음
- 이런 경우 class name.Companion 형태로 객체에 접근 가능
- companion object의 멤버가 다른 언어의 static 멤버 처럼 보일 수 있지만 아님
- companion object의 멤버는 실제 객체의 멤버임
- 슈퍼클래스도 가질 수 있는 클래스의 객체임
```kotlin
interface Factory<T> {
    fun create(): T
}
class MyClass{
    companion object : Factory<MyClass> {
        override fun create() : MyClass = MyClass()
    }
}
```
### object expressions vs object declaration
- object expressions는 즉시 초기화 되고 실행 된다.
- object declarations는 나중에 초기화 된다. (최초 접근 시)
- companion object는 클래스가 로드 될 때 초기화 됨, java static initializer와 매칭됨
