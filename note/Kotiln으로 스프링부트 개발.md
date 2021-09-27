## Kotlin으로 스프링부트 개발
### 왜 Kotlin으로 스프링부트를 개발하는가?
- 생산성 매우 증가
- 코틀린은 태생적으로 자바가 가지고 있는 문제? 개선사항을 해결함

### NullSafe
- 자바의 Optional, 객체가 null이라면 호출을 안하면 될일이지만, NullPointerException이 발생하는데, 코틀린은 객체가 null이면  null처리가 된다
```kotlin
data class Text{
  var text: String = "Hello World!!"
}
fun main(){
  var message: Text? = null
  println(message?.text) // print null
}
```
