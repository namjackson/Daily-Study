# item2. 변수의 스코프를 최소화하라.
상태를 정의할 때, 변수와 프로퍼티의 스코프는 최소화를 지향
- 프로퍼티 보다는 지역변수를 사용
- 최대한 좁은 스코프를 갖는 변수 활용

-> 좁은 스코프를 사용하는 이유는, 추적하고 관리하기 용이하기 떄문
코틀린의 경우, if, when, tryCatch, Elvis 표현식 등을 활용하여 구조분해 선언을 활용하면 좋다.

### 캡처링
소수를 구현하는 알고리즘 예제

- 시퀀스를 사용한 예제
```kotlin
val primes: Sequence<Int> = sequence {
    var numbers = generateSequence(2) { it + 1 }

    while (true) {
        val prime = numbers.first()
        yield(prime)
        numbers = numbers.drop(1)
            .filter {
                it % prime != 0
            }
    }
}
println(primes.take(10).toList()) 
// [2, 3, 5, 7. 11, 13, 17, 19, 23, 29]
```

- 잘못 활용한 예제 
```kotlin
val primes: Sequence<Int> = sequence {
    var numbers = generateSequence(2) { it + 1 }
    var prime: Int
    while (true) {
        prime = numbers.first()
        yield(prime)
        numbers = numbers.drop(1)
            .filter {
                it % prime != 0
            }
    }
}
println(primes.take(10).toList()) 
// [2, 3, 5, 6, 7, 8, 9, 10, 11, 12]
```

- 이상한 결과가 나온이유
	- yield(prime) 이후, Filter는 이후에 처리되는데, 그전에 while문 외부에 선언된 prime 참조하여, 마지막 prime값으로 계산되어 필터링됨


### 내생각
- 변수의 스코프 최소화는 기본적인 내용
- 가독성 및 관리의 용이성 외에도, 예측하지 못한 리스크를 최소화할 수 있다.