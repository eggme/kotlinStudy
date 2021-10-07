## 클래스와 오브젝트
- 코틀린에서의 모든 객체는 기본적으로 닫혀있다 (final 접근제한자)
- 그래서 클래스를 상속가능하게 바꾸려면 open 키워드를 클래스 앞에 붙여줘야한다.
- 자바와는 다르게 new 키워드가 필요 없다. 간단하게 다음과 같이 새로운 객체를 생성할 수 있다.
```kotlin
var person = Person()
```
- 위의 코드는 var 키워드를 사용했기 때문에, Person 타입의 가변 객체를 만든다. 가변객체는 말그대로 그 값을 바꿀 수 있는 객체를 의미한다.

### 생성자 초기화
- 자바는 아래의 코드처럼 클래스에서 필드들을 생성자에서 초기화하였다
```java
class Person {
    int roll_number;
    String name;
    
    public Student(int roll_number, String name){
        this.roll_number = roll_number;
        this.name = name;
    }
}
```
- 그래서 생성자의 인수의 이름이 클래스 속성의 이름과 유사하였다. 이러한 방법이 가독성에서도 좋았다. 또한 자바에서는 this 키워드를 사용해야했다.
- 코틀린은 훨씬 적은 코드로 속성들을 초기화할 수 있는 문법을 제공한다. 코틀린에서의 클래스 초기화는 다음과 같다.
```kotlin
class Student(var roll_number: Int, var name: String)

var student_A = Student(1,"승준");
```
- 속성의 초기화는 기본 생성자(기본 생성자는 클래스 헤더에 포함되어있다)에서 이루어진다. 물론, 클래스의 속성을 가변으로 할 것인지, 불변으로 할 것인지에 따라 var와 val 키워드를 모두 선택할 수 있다.
- 원한다면 기본생성자에 기본 값을 설정할 수도 있다
```kotlin
class Student constructor(var roll_number: Int, var name: String = "이승준")
```
- 만약 클래스에 기본 생성자가 있다면, 각각의 부차적인 생성자들은 직접적으로 혹은 다른 부차적인 생성자들을 통해 간접적으로 기본 생성자를 호출해야한다.
```kotlin
class Person(val name: String) {
    constructor(name: String, lastName: String) : this(name) {
        // 초기화를 위한 코드
    }
}
```
- 클래스의 속성뿐만 아니라 다른 것들을 초기화해야 할 때, init 블럭을 사용한다
```kotlin
class Student(var roll_number: Int, var name: String) {
    init {
        logger.info("Student initialized")
    }
}
```
- 가끔 의존성 주입을 통해 클래스의 속성들을 초기화할 때가 있는데, 만약 Dagger2(안드로이드 View Component들을 DI 해주는 듯?)를 사용해보았다면 클래스의 생성자에 바로 주입되는 객체들에 익숙할 것이다.
- 의존성 주입을 위해서는 @Inject 어노테이션을 constructor 키워드 앞에 추가하면 된다.
- 생성자가 어노테이션이나 접근제한자를 가지고 있다면 항상 constructor 키워드를 사용해야 한다.
```kotlin
class Student @Inject constructor(compositeDisposable): CompositeDisposable { }
```
- 클래스를 상속할 때는 부모클래스 또한 초기화해줘야한다. 이 작업 또한 코틀린에서는 매우 간단하다.
```kotlin
class Student constructor(var roll_number: Int, var name: String): Person(name)

class Student: Person {
    constructor(var name: String): super(name) 
    constructor(var roll_number: Int, var name: String) : super(name)
}
```

### 데이터 타입 변환 

### 클래스 속성 순회
- 코틀린에서도 리플렉션을 이용하면 런타임에 프로그램의 구조, 클래스 수정자, 함수 그리고 속성을 분석할 수 있다
```kotlin
class Student constructor(var roll_number: Int, var full_name: String)

fun main(args: Array<String>){
        var student = Student(2013001, "Aanand Shekhar Roy")
        for (property in Student::class.memberProperties){
                println("${property.name} = ${property.get(student)}")
        }
}

open class Person{
    val isHuman: Boolean = true
}
```
- Person 클래스를 사용하여 Student 클래스를 확장한 다음 이전에 memberProperties 메소드에서 사용한 것과 동일한 코드를 사용하면 출력코드는 다음과 같다
```
full_name = Aanand Shekhar Roy
roll_number = 2013001
isHuman = true
```
- 따라서 Student 클래스에만 선언된 필드를 반복하는 경우 declaredMemberProperties 함수가 필요하다. 다음은 declaredMemberProperties의 예제이다.
```kotlin
        for (property in Student::class.declaredMemberProperties){
                println("${property.name} = ${property.get(student)}")
        }
```
```
full_name = Aanand Shekhar Roy
roll_number = 2013001
```

### 인라인 속성
- 코틀린의 장점 중 하나는 함수를 다른 함수의 매개변수로 사용할 수 있다는 것이다. 그러나 그것은 객체이기 때문에 메모리 오버헤드가 발생할 수 있다. (모든 인스턴스는 힙 메모리의 공간을 차지하고, 또한 인자로 넘어간 함수를 호출하는 또 다른 메소드가 필요하기 때문이다)
- 이러한 상황을 인라인 함수를 사용하여 개선할 수 있다. 인라인 키워드는 함수 내용이 호출 위치에 직접 삽입됨을 의미한다. 이는 함수를 호출할 때 발생되는 오버헤드를 줄이는 데 도움이 된다.
- 마지막으로 inline 키워드는 backing field가 없는 변수와 해당 접근자와 함께 사용될 수 있다.
- 속성의 접근자를 인라인으로 만드는 예제를 따라해보자
```kotlin
fun main(args: Array<String>) {
    var a = x()
    a.valueIsMaxedOut = false
    println(a)
}

class x {
    companion object {
        val CONST_MAX = 3
    }
    
    var someValue = 3
    
    var valueIsMaxedOut: Boolean
        inline get() = someValue == CONST_MAX
        inline set(value) {
            println("Value Set!")
        }
}
```
- 속성 전체를 인라인으로 정의할수도 있다
```kotlin
    inline var alueIsMaxedOut: Boolean
        get() = someValue == CONST_MAX
        set(value) {
            println("Value Set!")
        }
```




