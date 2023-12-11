## ✨ enumerated()에 대해 알아보자 !
> 자료구조 문제를 풀던 중, 최초 stack의 인덱스 값을 활용해야하는 문제가 나왔다. 인덱스를 dictionary의 key값으로 저장해놓고, 사용을 하려고 하다가 배열의 enumerated() 함수를 사용하면 쉽게 배열의 인덱스를 가져올 수 있다는 것을 알았다.

</br>
</br>

<img width="818" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/97f430b7-043e-4000-8c42-b66402e19a6e">

공식 문서의 정의는 위와 같다. </br>

- `enumerated()`함수는 시퀀스의 각 요소에 대해 (index, element) 쌍을 생성하는데 사용된다.
- 이 때, index(n)는 0부터 시작해 연속적인 정수를 나타내고, element(x)는 시퀀스의 요소를 나타낸다. 
- 즉, 배열에서 `enumerated()` 함수를 사용하면, (index, element) **튜플 형식으로 구현된 리스트형**(EnumeratedSequence<>)이 반환된다.

</br>
</br>

### for문과 사용하기

```swift
let arr = ["one", "two", "three"]

for (index, num) in arr.enumerated() {
  print("\(index), \(num)")
}
```

위 코드의 결과는 아래와 같다. </br>

<img width="438" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/da5af113-690d-429b-ab54-2820a22ea0f5">

</br>
</br>

### 실제 사용 코드
> 내가 문제를 풀 때 사용했던 코드는 다음과 같다. </br>
> 한 칸씩 띄워져있는 숫자 배열을 입력받으면 각각의 숫자와 인덱스를 튜플 형태로 저장하는 코드이다.

```swift
var data = Array(readLine()!.split(separator: " ").enumerated()).map { ($0, Int($1)!) }
```

만약 입력 값으로 `1 2 3 4 5`를 입력했을 때 출력되는 결과는 `[(0, 1), (1, 2), (2, 3), (3, 4), (4, 5)]`가 된다.
