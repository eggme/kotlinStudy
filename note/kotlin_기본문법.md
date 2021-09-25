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
