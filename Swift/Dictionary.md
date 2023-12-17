## ✨ 딕셔너리(Dictionary) 타입 총정리
> Swift에서 딕셔너리 타입이 쓰이는 형태와 관련 매서드를 정리해본다. </br>
</br>

### 💡 딕셔너리 (Dictionaries)
> Swift의 Dictionary 타입은 Foundation의 `NSDictionary` 클래스와 연결된다.
- 딕셔너리 (dictionary)는 **순서와 상관없이** 콜렉션에 같은 타입의 **키(key)** 와 같은 타입의 **값(value)** 을 저장한다.
- 각 값은 딕셔너리 내부에서 값에 대한 식별자로 동작하는 *유니크한 키* 와 조합된다. -> 키 값은 중복불가
- 배열의 아이템과 다르게 딕셔너리의 아이템은 **특정 순서를 가지고 있지 않는다.**
- 특정 단어를 찾기위해 사전을 찾는 방법과 같이 **식별자를 기준을 값을 찾을 때** 딕셔너리를 사용한다.

</br>
</br>

### 💡 생성
> 딕셔너리 Key 타입은 집합의 값 타입과 같이 반드시 `Hashable 프로토콜`을 준수해야 한다. </br>
> 따라서 Value의 경우 Any 자료형을 사용할 수 있지만, Key 타입으로는 **Any를 사용할 수 없다.**

#### 1. 빈 딕셔너리 생성

배열처럼 초기화 구문을 사용하여 타입을 포함한 빈 Dictionary를 생성할 수 있다.

```swift
var namesOfIntegers = [Int: String]()

```
> 키의 타입은 Int이고, 값의 타입은 String이다.

위의 코드처럼 이미 타입정보를 제공한다면 **빈 딕셔너리 리터럴([:])** 를 사용해 빈 딕셔너리를 생성할 수도 있다.

```swift
namesOfIntegers[16] = "sixteen"
namesOfIntegers = [:]
```
</br>

#### 2. 딕셔너리 리터널로 생성

딕셔너리 리터럴은 Dictionary 콜렉션으로 하나 이상의 키-값 쌍으로 작성하는 짧은 작성법이다. </br>
딕셔너리 리터럴에서 각 키-값 쌍의 키와 값은 콜론으로 구분되고, 키-값 쌍은 콤마로 구분하고 대괄호로 둘러싸 리스트 형식으로 작성된다.

```swift
var airports: [String: String] = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]
```
> 딕셔너리 리터럴은 2개의 String: String 쌍을 포함한다.
> 키-값 타입은 airports 변수 선언 (딕셔너리는 String 키와 String 값만 가능) 타입과 동일하며 airports 딕셔너리에 2개의 초기 아이템을 포함하여 초기화 하는 것은 가능하다.

```swift
var airports = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]
```
> 배열과 마찬가지로 키와 값이 **일관된 타입을 갖는 딕셔너리 리터럴로 초기화**하는 경우 딕셔너리 타입을 작성할 필요가 없다.

</br>
</br>

### 💡 접근 및 수정

메서드와 프로퍼티 또는 서브 스크립트 구분을 사용하여 딕셔너리에 접근과 수정이 가능하다.

#### count와 isEmpty

배열과 마찬가지로 읽기 전용 count 프로퍼티로 Dictionary 에 아이템의 개수를 확인할 수 있다. </br>
count 프로퍼티가 0 과 같은지 아닌지 Bool타입의 isEmpty 프로퍼티를 이용하여 확인할 수 있다.

```swift
print("The airports dictionary contains \(airports.count) items.

if airports.isEmpty {
    print("The airports dictionary is empty.")
} else {
    print("The airports dictionary is not empty.")
}
```

</br>

#### 서브 스크립트 구문

서브 스크립트 구문을 사용하여 딕셔너리에 새로운 아이템을 **추가**할 수 있다. </br>
이 때 적절한 키의 타입을 서브 스크립트 인덱스로 사용하고 적절한 값의 타입을 할당해야 한다. </br>

```swift
airports["LHR"] = "London"
```
</br>

특정 키를 서브 스크립트 구문으로 사용하여 값을 **변경**할 수도 있다. </br>

```swift
airports["LHR"] = "London Heathrow"
```

</br>

#### updateValue() 매서드

서브 스크립트 외에 딕셔너리의 `updateValue()` 메서드를 사용하여 특정 키에 값을 설정하거나 업데이트 할 수 있다. </br>
위의 서브 스크립트와의 공통점은 해당 키에 값이 존재하지 않으면 값을 설정 하거나 해당 키에 값이 존재하면 값을 업데이트 한다는 것이다. </br>
그러나 서브 스크립트와 다르게 `updateValue()` 메서드는 업데이트 수행 후에 **이전 값**을 반환한다.</br>
이를 통해 **업데이트가 발생했는지 여부**를 알 수 있다. </br>

```swift
if let oldValue = airports.updateValue("Dublin Airport", forKey: "DUB") {
    print("The old value for DUB was \(oldValue).")
}

// The old value for DUB was Dublin.
```

</br>

`updateValue()` 메서드는 딕셔너리의 값 타입의 **옵셔널 값**을 반환한다. </br>
예를 들어 딕셔너리에 String 값을 저장하면 **String?** 타입 또는 **옵셔널 String**을 반환하고, 해당 키에 존재한 업데이트 전에 값 또는 존재한 값이 없었을 때는 `nil` 을 반환한다. </br>

```swift
print(airports.updateValue("ICN", forKey: "Incheon Airport"))

// nil
```

</br>

#### 삭제

딕셔너리의 해당 키에 `nil` 값을 할당하여 키-값 쌍을 서브 스크립트 구문을 사용하여 삭제할 수 있다. </br>

```swift
airports["APL"] = "Apple International"
airports["APL"] = nil

// APL 삭제
```

</br>

removeValue() 매서드를 사용해 키-값 쌍을 삭제할 수도 있다. </br>
이 때, 키-값 쌍이 존재하면 삭제하고 **삭제된 값을 반환**하거나 값이 존재하지 않으면 `nil` 을 반환한다. </br>

```swift
if let removedValue = airports.removeValue(forKey: "DUB") {
    print("The removed airport's name is \(removedValue).")
} else {
    print("The airports dictionary does not contain a value for DUB.")
}

// The removed airport's name is Dublin Airport.
```

</br>
</br>

### 💡 반복

- `for-in` 루프로 딕셔너리에 키-값 쌍을 반복할 수 있다.
- 딕셔너리의 각 아이템은 `(key, value) 튜플`로 반환되고 튜플의 멤버를 임시 상수 또는 변수로 분리할 수 있다.

```swift
for (airportCode, airportName) in airports {
    print("\(airportCode): \(airportName)")
}

// LHR: London Heathrow
// YYZ: Toronto Pearson
```

</br>

- key값 모두 나열하기

```swift
for airportCode in airports.keys {
    print("Airport code: \(airportCode)")
}

// Airport code: LHR
// Airport code: YYZ
```

- value값 모두 나열하기

```swift
for airportName in airports.values {
    print("Airport name: \(airportName)")
}

// Airport name: London Heathrow
// Airport name: Toronto Pearson
```

</br>

Swift의 Dictionary 타입은 정의된 순서를 가지고 있지 않으므로, 특정 순서로 딕셔너리의 키 또는 값을 반복하려면 keys 또는 values 프로퍼티에 `sorted()` 메서드를 사용해야한다.

```swift
airports.keys.sorted()
airports.values.sorted()
```

</br>
</br>

### 💡 검색

- 딕셔너리 요소를 검색할 때에는 클로저를 이용해 검색하는데, 이 때 클로저의 파라미터 타입은 지정한 딕셔너리의 타입을 갖는 튜플이어야 하고, 반환타입은 반드시 **Bool**이어야 한다.
- 즉, 만약 [String: String]타입의 딕셔너리일 경우 ((String, String)) -> Bool 과 같이 클로저를 작성해야 한다.

```swift
var dict = ["Jenny": 100, "Jisoo": 120, "Rose": 90, "Risa": 80]

let condition: ((String, Int)) -> Bool = {
  $0.0.contains("J")
} 
```

</br>

```swift
print(dict.contains(where: condition))
print(dict.first(where: condition))
print(dict.filter(condition))

// true
// Optional((key: "Jenny", value: 100))
// ["Jisoo": 120, "Jenny": 100]
```

#### contains(where: )
>  해당 클로저를 만족하는 요소가 하나라도 있을 경우 true, 아닐 경우 false

#### first(where: )
> 해당 클로저를 만족하는 첫 번째 요소 튜플로 리턴 </br>
> 딕셔너리는 순서가 없기 때문에, 호출할 때마다 값이 바뀔 수 있다.


#### filter
> 해당 클로저를 만족하는 요소만 모아서 새 딕셔너리로 리턴
