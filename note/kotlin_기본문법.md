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

#### Collections
- 람다식을 이용해서 컬렉션에 filter, map 등의 연산 가능
```kotlin
val fruits = listOf("banana", "avocado", "apple", "kiwi")
fruits.filter { it.startsWith("a") }
      .sortedBy {it}
      .map {it.toUpperCase()}
      .forEach{ println(it) }

```
