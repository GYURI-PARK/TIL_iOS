# 👀 Subscribe

> **subscriber**은 `Publisher 프로토콜`에 정의되어 있는 `subscribe(_ :)`함수를 이용해 **publisher**를 구독합니다 ! 

<img width="445" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/4b078e4f-78aa-4329-8b6d-7e72a6c78183">

## publisher가 subscriber를 구독하는 과정

### 1. publisher에 딸린 `subscribe(_:)` 함수를 통해 subscriber의 구독을 요청합니다.

<img width="440" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/4cc23987-3e4e-4728-9929-dda626ee55e8">

### 2. subscribe(_:)의 구현부에서 `receive(subscriber:)` 함수를 호출합니다.

<img width="582" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/1b63af41-e86c-4bda-96ed-64d9c499e904">

`receive(subscriber:)` 함수는 ***Publisher라면 모두 구현(implement)해야하는 함수***로서 프로토콜 **Publisher**에 정의되어 있습니다.

### 3. 호출된 receive(subscriber:)함수에서 publisher와 subscriber을 연결해줍니다.

- 우선 구독권 (**subscription**)을 만듭니다.

<img width="521" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/2b976e2b-78b0-43c4-82ad-21f4d524da79">

- 이렇게 만들어진 *subscription을 subscriber에게 전달하기 위해* subscriber의 `receive(subscription:)`을 호출한다. 해당 함수는 **Subscriber 프로토콜**에 정의되어 있습니다.

<img width="608" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/a72dbfa1-b3d6-4922-81b8-b19ce1b64a85">

→ 이렇게 publisher와 subscriber는 subscription을 통해 연결됩니다.

### 4. receive(subscription:)에서는 Subscription 프로토콜에 정의된 `request(_:)`을 호출하여 값을 최대로 얼만큼 받을 것인지 설정합니다.

> 이 때 함수의 parameter type은 **Demand**  !

<img width="352" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/3ce4552f-22a6-4709-95d5-33c2ea36c0ef">

3개 이상으로 값을 들어올 수 없습니다.

### Subscription의 역할

- **publisher**에서 새로운 값이 발생했을 때 **publisher**와 **subscriber**을 중재하고 **subscriber**가 요청한 것보다 더 많은 값을 받지 않도록 보장합니다.
- **subscriber**의 유지 및 해제를 관리합니다.

### 5. subscriber의 `receive(_:)`를 통해 publisher가 방출하는 새로운 값들을 전달할 수 있습니다.

이 함수는 여러번 호출될 수 있습니다.

4번에서 `request(_:)`의 parameter 타입이 **Demand**였던 것과 마찬가지로 `receive(_ :)`의 parameter 타입도 **Demand**이다. 따라서 처음에 subscriber가 받을 수 있는 최대 개수를 정하더라도 새 값을 받을 때마다 최대 값을 조절할 수 있습니다.

### 6. 만약 publisher가 더 이상 값을 생성하지 않거나 error가 발생한다면 subscriber의 `receive(completion:)`을 호출해 종료를 알립니다.

</br>

### 전체 흐름도

<img width="632" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/90f93458-9b21-46a0-b74c-0077391eff41">
<img width="633" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/bb0f11b3-bba0-40ee-aca7-3aed3744749a">

## 예제 코드

```swift
example(of: "Custom Subscriber") {
// 1: 1부터 6까지의 값을 방출하는 publisher를 생성합니다.
let publisher = (1...6).publisher
  
// 2: Subscriber 을 상속받는 custom subscriber을 만듭니다.
final class IntSubscriber: Subscriber {
  
  // 3: type aliases를 통해 위의 publisher의 Output과 Failure 타입과 일치하는 Input과 Failure 타입을 각각 정의합니다. 
  typealias Input = Int
  typealias Failure = Never
  
  // 4: publisher에서 생성된 subscription을 전달받을때 호출되는 함수입니다.
  func receive(subscription: Subscription) {
    //4-1: 해당 함수 안에서는 subscription의 .request(_:) 를 호출을 통해 최대 3개의 값을 수신할 것임을 알립니다.
    subscription.request(.max(3))
  }

// 5: 각 값을 수신할때 호출되는 함수 입니다.
  func receive(_ input: Int) -> Subscribers.Demand {
    //5-1: 받은 값을 print 합니다
    print("Received value", input)
    //5-2: .none 을 반환하여 subscriber의 수요 조정이 없음을 나타냅니다. 즉 .max(0) 과 같은 의미입니다
    return .none
  }

  // 6: 완료 이벤트를 받을때 호출되는 함수입니다.
  func receive(completion: Subscribers.Completion<Never>) {
    //6-1: 완료 이벤트를 프린트 합니다.
    print("Received completion", completion)
  }
}
  
//7: subscriber를 생성합니다.
let subscriber = IntSubscriber()
//8: publisher에 subscriber를 붙입니다.
publisher.subscribe(subscriber)
}
```
</br>

출력

<img width="623" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/d3626d24-9f85-4a9d-87e8-cd59b6fac729">
