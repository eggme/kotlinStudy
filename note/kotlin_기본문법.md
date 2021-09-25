## 코틀린 기본 문법
#### 코틀린
- 간결하고, 안전한 언어, 호환성이 좋음
- 자동으로 만들어주는 Getter, Setter
- Java의 람다보다 더 간결한 람다식
- 싱글톤 객체를 언어에서 지원

#### 함수 정의
- fun 키워드로 함수 정의
```kotlin
fun sum(a:Int, b:Int) : Int {
  return a + b
}
```
- 함수 자체가 식인 경우는 return 생략 가능
```kotlin
fun sum(a:Int, b:Int) = a + b
```
- 자바의 void -> 코틀린 Unit (생략가능)


#### 변수 정의
- val -> 읽기 전용 변수 (Java의 final)
```kotlin
val a:Int = 1   // 즉시 할당
val b = 2       // 'Int' type 추론
val c:Int       // 컴파일 오류, 초기화가 필요함
c=3             // 컴파일 오류, 읽기 전용
```
- var -> Mutable변수
```kotlin
var x = 5
x += 1
```

#### 주석
- Java와 Javascript와 동일

#### 문자열 템플릿
- String Interpolation
```kotlin
var a = 1
val s1 = "a is $a"

a = 2
val s2 = "${s1.replace("is", "was")}, but now is $a"
```

#### 조건문
```kotlin
fun maxOf(a:Int, b:Int) : Int {
  if(a > b){
    return a
  } else {
    return b
  }
}
```
- 조건식으로도 가능
```kotlin
fun maxOf(a:Int, b:Int) = if(a>b) a else b
```
#### nullable
- 값이 null일 수 있는 경우 타입에 nullable 마크를 명시해야함
```kotlin
fun parseInt(str: String): Int? {
  ...
}
```
- nullable 타입의 변수를 접근 할 때는 반드시 null 체크를 해야 함
- 그렇지 않으면 컴파일 오류가 발생
```kotlin
fun printProduct(arg1: String, arg2: String) {
  val x : Int? = parseInt(arg1)
  val y : Int? = parseInt(arg2)
  
  if(x != null && y != null){
      println( x * y )
  } else {
      println("either '$arg1' or '$arg2' is not a number")
  }
}
```

#### 자동 타입 변환
- 타입 체크만 해도 자동으로 타입변환이 됨
```kotlin
fun getStringLength(obj: Any) : Int? {
    if(obj is String){ // is 로 비교하여 통과한다면
        return obj.length // 'obj'가 자동으로 String 타입으로 변환 됨
    }
    return null
}
```

#### when 표현식
```kotlin
fun describe(obj: Any) : String =
    when (obj) {
        1           -> "One"
        "Hello"     -> "Greeting"
        is Long     -> "Long"
        !is String  -> "Not a String"
        else        -> "Unknown"
    }
```

#### ranges
- In 연산자를 이용하여 숫자 범위를 체크 가능
```kotlin
val x = 3
if (x in 1..10) { // 1~10 사이에 있는지 체크, if문이나 for문에서 사용 가능
    println("fits in range")
}
```
- range를 이용한 for loop
```kotlin
for (x in 1..5){
    print(x)
}
```

#### Collections
- 컬렉션도 in으로 loop 가능
```kotlin
val items = listOf("apple", "banana", "kiwi")
for(item in items){
    println(item)
}
```
- in으로 해당 값이 collection에 포함되는지도 체크 가능
```kotlin
val items = setOf("apple", "banana", "kiwi")
when {
    "orange" in items     -> println("juicy")
    "apple" in items      -> println("apple is fine too")
}
```
- 람다식을 이용해서 컬렉션에 filter, map 등의 연산 가능
```kotlin
val fruits = listOf("banana", "avocado", "apple", "kiwi")
fruits.filter { it.startsWith("a") }
      .sortedBy {it}
      .map {it.toUpperCase()}
      .forEach{ println(it) }
```

### Kotlin - Basic Type
- 기본 타입
  - 코틀린에서 모든 것은 객체임
  - 모든 것에 멤버 함수나 프로퍼티를 호출 가능하다는 의미에서..
```kotlin
fun test(){
    println(1.toString())
}
```
- 숫자 (Numbers)
  - Java의 숫자형과 거의 비슷하게 처리
  - Kotlin에서 Number는 클래스임, java의 primitive type에 직접 접근할 수 없음
  - Java에서 숫자형이던 char가 kotlin에서는 숫자형이 아님
- 리터럴 (Literal)
  - 10 진수 : 123 (Int, Short)
  - Long : 123L
  - Double : 123.5, 123.5e10
  - Float : 123.5f
  - 2진수 : 0b00001011
  - 8진수 : 미지원
  - 16진수 : 0X0F
- Underscores in numeric literals
```kotlin
val oneMillion = 1_000_000
val creditCardNumber = 1234_5678_9012_3456L
val socialSecurityNumber = 999_99_9999L
val hexBytes = 0xFF_EC_DE_5E
val bytes = 0b11010010_01101001_10010100_10010010
```
#### Representation
- Java플랫폼에서 숫자형은 JVM primitive type으로 저장됨
- Nullable이나 제네릭의 경우에는 박싱됨
- 박싱된 경우 idendtity를 유지하지 않음

#### Explicit Conversions
- 작은 타입은 큰 타입의 하위 타입이 아님, 즉 작은 타입에서 큰 타입으로 대입이 안됨
```kotlin
val a: Int = 1 // A boxed Int
// val b: Long = a // 오류
val b: Long = a.toLong()
// println(a == b) // 오류
```
- 명시적으로 변환을 해주어야함
```kotlin
val i: Int = b.toInt() // OK
```
#### 문자(Characters)
- Char은 숫자로 취급되지 않음
```kotlin
fun check(c: Char) {
    if( c == 1) {} // ERROR
}
fun check(c: Char){
    if(c == 'a') {}  // OK
}
println('0'.toInt()) // print 48
```
#### 배열
- 배열은 Array클래스로 표현됨
- get,set ([] 연산자 오버로딩됨)
- size 등 유용한 멤버 함수 포함
```kotlin
var array: Array<String> = arrayOf("코틀린", "강좌")
println(array.get(0))
println(array[0])
println(array.size)
```
##### 배열 생성
- Array의 팩토리 함수를 이용
- arrayOf()등의 라이브러리 함수 이용
```kotlin
val b = Array(5, {i -> i.toString()})
val a = arrayOf("0", "1", "2", "3", "4")
```
##### 특별한 Array 클래스
- Primitive 타입의 박싱 오버헤드를 없애기 위한 배열
- IntArray, ShortArray
- Array를 상속한 클래스들은 아니지만, Array와 같은 메소드와 프로퍼티를 가짐
- size등 유용한 멤버 함수 포함
```kotlin
val x: IntArray = intArrayOf(1,2,3)
x[0] = 7
println(x.get(0))
println(x[0])
println(x.size)
```
##### 문자열
- String 클래스로 표현
- String은 characters로 구성됨
- s[i] 와 같은 방식으로 접근 가능 (immutable이므로 변경 불가)

##### 문자열 리터럴
- __escaped string ("Kotlin")__
  - 전통적인 방식으로 Java String과 거의 비슷
  - Backslash를 사용하여 escaping 처리
- __raw string ("""Kotlin""")__
  - escaping 처리 필요 없음
  - 개행이나 어떠한 문자 포함 가능
```kotlin
val s = "Hello, world!\n"

val s = """
"'이것은 코틀린의
  raw String
  입니다.'"
  """
```

## Control Flow
- 자바랑 상당히 달라서 이 부분을 잘 공부해야함
- switch case -> when, if문의 값 반환 등...
#### if else 문
- Java와 거의 유사함
```kotlin
var max = a
if(a < b) max = b

var max: Int
if(a > b){
    max = a
} else {
    max = b
}
```
- If문이 식으로 사용되는 경우 값을 반환함
- If식의 경우 반드시 else를 동반해야 함
```kotlin
val max = if(a > b) a else b
```
- If식의 branches들이 블록을 가질 수 있음 {...}
- 블록의 마지막 구분이 반환 값이 됨
```kotlin
val max = if(a > b){
    print("Chooes a")
    a
} else {
    print("Chooes b")
    b
}
```
- 삼항연산자가 없음 (If문이 삼항연산자 역할을 잘 해내기 때문에)
```java
int max = (a > b) ? a : b;
```
```kotlin
val max = if(a > b) a else b
```
#### When
- when문은 C계열 언어의 switch문을 대체함
- when문은 각각의 branches의 조건문이 만족할 때 까지 위에서부터 순차적으로 인자를 비교함
```kotlin
when(x) {
  1 -> print("x == 1")
  2 -> print("x == 2")
  else -> {
      print("x is neither 1 nor 2")
  }
}
```
- when 문이 식으로 사용된 경우에는 조건을 만족하는 branch의 값이 전체 식의 결과 값이 됨
- else의 경우 다른 branch들의 조건이 만족되지 않을 때 수행됨
- when이 식으로 사용될 경우 else문이 필수
- when이 식으로 사용된 경우 컴파일러가 else문이 없어도 된다는 것을 입증할 수 있는 경우에는 else를 생략가능
```kotlin
var res = when(x){
    100   -> "A"
    90    -> "B"
    80    -> "C"
    else  -> "F"
}

var res = when(x) {
  true    -> "맞다"
  false   -> "틀리다"
}
```
- 여러 조건들이 같은 방식으로 처리될 수 있는 경우, branch의 조건문에 콤마를(,) 사용하여 표기하면 됨
```kotlin
when(x) {
  0,1,2,3,4   -> print("x = 0~4")
  else        -> print("otherwise")
}
```
- Branch의 조건문에 함수나 식을 사용할 수 있음
```kotlin
when(x){
  parseInt(x)   -> print("s encodes x")
  1 + 3         -> print("4")
  else          -> print("s does not encode x")
}
```
- range나 collection에 in이나 !in으로 범위 등을 검사할 수 있음
```kotlin
val validNumbers = listOf(3,6,9)
when(x){
  in validNumbers     -> print("x is valid")
  in 1..10            -> print("x is in the range")
  !in 10..20          -> print("x is outside the range")
  else                -> print("none of the above")
}
```
- Is나 !Is를 이용하여 타입도 검사할 수 있음  (이 때, 스마트 캐스트가 적용됨)
```kotlin
fun hasPrefi(x: Any) = when(x) {
    is String -> x.startsWith("prefix")
    else  ->  false
}
```
- when은 if-else if 체인을 대체할 수 있음
- when에 인자를 입력하지 않으면 논리연산으로 처리됨
```kotlin
when{
    x.isOdd()     -> print("x is odd");
    x.isEven()    -> print("x is even");
    else          -> print("x is funny")
}
```
#### For loops
- for문은 iterator을 제공하는 모든 것을 반복할 수 있음
```kotlin
for(item in collection)
    print(item)
```
- for문의 Body가 블록이 올 수도 있음
```kotlin
for(item in collection){
  print(item.id)
  print(item.name)
}
```
- for문을 지원하는 iterator의 조건
  - 멤버함수나 확장함수 중에
    - iterator()를 반환하는 것이 있는 경우
      - next()를 가지는 경우
      - hasNext(): Boolean를 가지는 경우
    - 위의 3함수는 operator로 표기 되어야함 
```kotlin
val myData = MyData()
    for(item in myData){
        print(item)
    }
}
class MyIterator {
    val data = listOf(1,2,3,4,5)
    var idx = 0
    operator fun hasNext(): Boolean{
        return data.size > idx
    }
    operator fun next(): Int{
        return data[idx++]
    }
}
class MyData {
    operator fun iterator(): MyIterator{
        return MyIterator()
    }
}
```
- 배열이나 리스트를 반복할 때, index를 이용하고 싶다면 indices를 이용하면 됨
```kotlin
val array = arrayOf("가", "나", "다")
for(i in array.indices) {
    println("$i: ${array[i]}")
}
```
- Index를 이용하고 싶을 때, withIndex()를 이용할 수 있음
```kotiln
val array = arrayOf("가", "나", "다")
for((index, value) in array.withIndex()){
    println("$index: ${value}")
}
```
#### while loops
- while, do-while문은 java와 거의 같음
- do-while문에서 body의 지역변수를 do-while문의 조건문이 참조할 수 있음
```kotiln
while(x>0){
  x--
}
do{
  val y = retrieveData()
}while(y != null) // 보면 do안쪽의 scope의 변수를 사용할 수 있음
```

## Package, Return and Jumps
#### 패키지
- 소스 파일은 패키지 선언으로 시작 됨
- 모든 콘텐츠(클래스, 함수, ...)는 패키지에 포함 됨
- 패키지를 명세하지 않으면 이름이 없는 기본 패키지에 포함됨
- 기본 패키지
  - 기본으로 import 되는 package가 있음
  - 플랫폼 별로 import되는 package도 다른 부분도 있음
- Imports
  - 기본으로 포함되는 패키지 외에도, 필요한 package들을 직접 import할 수 있음
```kotlin
import foo.Bar
import foo.*
// foo.Bar
// bar.Bar 이름이 충돌 나는 경우 'as' 키워드로 로컬 리네임 가능
import bar.Bar as bBar
```

## Return and Jumps
#### 3가지 Jump 표현식
- return : 함수나 익명 함수에서 반환
- break : 루프를 종료시킴
- continue : 루프의 다음 단계로 진행
```kotiln
fun sum(a: Int, b:Int): Int{
    println("a: $a, b: $b")
    return a + b
}
```
```kotiln
for(x in 1..10){
  if(x > 2){
      break
  }
  println("x: $x")
}
```
```kotiln
for(x in 1..10){
  if(x < 2){
      continue
  }
  println("x : $x")
}
```
##### Label로 Brean and Continue
- 레이블 표현 : label@, abc@, fooBar@
  - 식별자 + @ 형태로 사용
```kotlin
loop@ for(i in 1..10){
    println("--- i:$i ---")
    
    for(j in 1..10){
        println("j: $j")
        if(i + j > 12){
            break@loop
        }
    }
}
```
```kotlin
loop@ for(i in 1..10){
    println("--- i: $i ---")
    
    for(j in 1..10){
        if(j < 2){
            continue@loop
        }
        println("j: $j")
    }
}
```
##### Label로 Return
- 함수가 중첩되어 있을 때의 return은 가장 가까운 scope의 함수를 return 한다 __(람다는 함수가 아니기 때문에, 의도하지 않은 결과가 나올 수 있음)__
- 코틀린에서 중첩될 수 있는 요소들
  - 함수 리터럴
  - 지역함수
  - 객체 표현식
  - 함수
```kotlin
fun foo(){
  var ints = listOf(0,1,2,3)
  
  ints.forEach(fun(value: Int) {
      if(value == 1) return
      print(value)
  })
  print("End")
}
```
- 람다식에서 return 할 때 주의사항
  - 람다식에서 return시 nearest enclosing 함수가 return됨
  - 람다식에서 대해서만 return 하려면 label을 이용해야 함
```kotlin
fun foo(){
  var ints = listOf(0,1,2,3)
  ints.forEach {
    if(it == 1) return
    println(it)
  }
  print("End")
}
```
```kotlin
fun foo3(){
  var ints = listOf(0,1,2,3)
  ints.forEach label@{
    if(it == 1) return@label
    println(it)
  }
  print("End")
}
```
- __암시적 레이블__
  - 람다식에서만 return 하는 경우 label을 이용해서 return 해야 함
  - 직접 label을 사용하는 것 보다 암시적 레이블이 편리함
  - 암시적 레이블은 람다가 사용된 함수의 이름과 동일함
```kotlin
fun foo4(){
  var ints = listOf(0,1,2,3)
  
  ints.forEach {
      if(it == 1) return@forEach
      print(it)
  }
  print("End")
}
```
- 레이블 return시 값을 반환할 경우
  - return@label 1 형태로 사용
  - return + @label + 값
```kotlin
fun foo(): List<String> {
    var ints = listOf(0,1,2,3)
    val result = ints.map {
        if(it == 0) {
            return@map "zero"
        }
        "number $it"
    }
    return result
}
```













