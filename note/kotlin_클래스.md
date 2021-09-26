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







