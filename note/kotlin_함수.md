### 변수인수를 함수로 전달
- 변수인수를 함수에 전달해야하는 시나리오는 무수하다. 코틀린에서는 vararg 수정자를 사용하여 변수인수를 함수에 전달할 수 있다.
- 함수에 가변 개수의 인수를 전달하는 방법을 보여주는 다음 단계들을 살펴보자
```kotlin
fun main(args: Array<String>){
  someMethod("as", "you", "know", "this", "works")
}

fun someMethod(vararg a: String) {
    for(a_ in a){
      println(a_)
    }
} 
```
- 이미 값배열을 가지고 있다면 * 스프레드 연산자를 사용하여 직접 값을 전달할 수 있다.
```kotlin
fun main(args: Array<String>){
  var list - arrayOf("as", "you", "know", "this", "works")
  someMethod(*list)
}

fun someMethod(vararg a: String) {
    for(a_ in a){
      println(a_)
    }
} 
```
- 기본적으로 vararg는 컴파일러에게 전달받은 인수를 배열로 래핑하도록 한다
