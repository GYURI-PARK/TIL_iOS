## Combine 기초

### Combine이란?

> Customize handling of asynchronous events by combining event-processing operators </br>

💡 즉, Combine 사용의 궁극적 목적 = 비동기 이벤트를 잘 처리하기 위해 !!!

</br>
</br>

비동기 프로그래밍을 할때는 재현이나 추적 및 수정이 어렵다는 단점이 있습니다. 이러한 문제의 원인은 각각이 **고유한 인터페이스를 가지는 비동기 API를 사용하기 때문**입니다.

<img width="623" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/6a895dce-8cf8-4322-8100-c74aaeac9906">

Combine에서는 이렇게 비동기로 동작하는 모든 유형을 통합해서 사용할 수 있습니다.

Combine에서 알아야할 중요한 개념에는 **Publisher**, **Operator**, **Subscriber**이 있습니다

- Publisher : 값을 시간에 따라 변경할 수 있는 Publisher를 선언하고
- Subscribers : 이러한 값을 Publisher에서 Subscribers가 받을 수 있도록 합니다.
- Operator : 연산자로, publisher가 내보낸 값에 대해 연산을 수행하는 메소드

<img width="616" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/ffbf8da1-c55e-489f-a6c0-9e3dfc90ef45">

</br>
</br>

### Combine은 언제사용할까?

<aside>
💡 다양한 입력에 반응하는 무언가를 설정하는 경우

</aside>

#### Combine으로 할 수 있는 대표적인 작업들

- 필드에 입력한 값이 유효한 경우에만 submit 버튼이 활성화되도록 설정
- 비동기작업을 수행하고 반환된 값을 사용하여 View를 업데이트할 방법과 대상을 선택 가능
- 사용자가 텍스트 필드에 동적으로 입력하고 입력한 내용을 기반으로 사용자 인터페이스 보기를 업데이트

</br>
</br>

### Publisher

> Publisher는 어떠한 요소(element)를 시간 흐름에 따라 방출합니다.

<img width="663" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/cda76ed9-3353-4aa4-82e4-670c87ddff68">

위의 그림처럼 1초에 `4`를 방출하고, 5초에 `8`, 8초에 `15`라는 값을 방출 → 시간의 흐름에 따라 이벤트가 방출 </br>

이렇게 방출된 값은 하나 또는 그 이상의 Subsciber에 전달됩니다.
</br>

- 한 개의 Subscriber

<img width="655" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/5a3c0812-c901-4cec-84cc-ca2258949a0d">


- 다수의 Subscriber

<img width="641" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/34bb0f10-361e-4c72-b995-d5691c9b881a">


#### Publisher<Int, Never>

> **Output**은 publisher가 방출하는 값의 종류로 `Int` 타입을 가지고, **Failure**은 publisher가 방출할 수 있는 에러의 종류입니다.

<img width="640" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/73889198-1dbb-4092-8690-e8e03bb73b51">

이 때 **Subscriber**의 `Input & Failure`의 associated type은 **Publisher**의 `Output & Failure`타입과 일치해야 합니다.

</br>
</br>

### Protocol

**publisher**는 **subscriber**를 받아들이기(accept)위해 `receive(subscriber: )`를 구현하고 해당 함수는 `Publisher protocol`에 정의되어 있습니다.

- **receive(subscription: )** : subscriber에게 Subscription 인스턴스를 전달해줍니다. 이것을 이용해서 subscriber은 publisher의 elements를 요구하거나, 더 이상 값을 받지 않겠다고 할 수도 있다. Subscription은 subscriber와 publisher의 연결을 나타낸다.
- **receive(_:)** :  publisher에서 생성된 element를 subscriber로 전달하기 위해 쓰인다.
- **receive(completion: )** : subscriber에게 이벤트 방출(publishing)이 정상적으로 또는 에러로 인해 끝났음을 알려준다.

</br>
</br>

### Publisher의 종류

#### Convenience Publisher

- Future : 실패하거나 하나의 값을 방출한 후 완료됩니다.
- Just : 각각의 subscriber에게 딱 한 번 값을 방출하고 완료됩니다.
- Empty : 어떤 값도 내보내지 않습니다. 즉시 종료될 수도 있습니다.
- Fail : 특정한 error와 함께 즉시 종료됩니다.

</br>
</br>

### Subscriber

> Publisher에서 나오는 요소의 흐름을 받습니다.

Publisher에서는 Output과 Failure가 있듯, Subscriber에는 Input과 Failure가 있습니다.

**Publisher**

- Output : `publisher`가 방출하는 **값**의 종류
- Failure : `publisher`가 방출할 수 있는 **에러**의 종류

**Subscriber**

- Input : `subscriber`가 받는 **값**의 종류
- Failure : `subscriber` 받을 수 있는 **에러**의 종류

</br>
</br>

### Protocol

#### 1. receive(subscription:)
> subscriber와 publisher를 연결하기 위해 subscribe(_ :) 함수를 부르고 난 뒤에 publisher 호출하는 함수
> 
<img width="653" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/91706038-8f65-45c6-825b-e55fbb5bb1df">

이 함수에서는 subscriber에게 Subscription 인스턴스를 전달해줍니다.

`Subscription` 은 `subscriber`와`publisher`의 연결을 나타냅니다! 이것을 통해서 `subscriber`은 `publisher`의 `elements`를 요구하거나, 아래 코드처럼 더 이상 값을 받지 않겠다고 할 수도 있습니다.

```swift
subscription.cancel()
```

이렇게 `subscription`을 명시적으로 취소하지 않은면 `publisher`가 완료될때까지 또는 일반적인 메모리 관리로 인해 저장된 `subscription`이 초기화 되지 않을때까지 계속 유지됩니다.

</br>

#### 2. receive(_:)

> **subscriber**의 첫번째 요청(initial demand)이 이뤄지고 나면 **publisher**가 새롭게 발행된 element들을 **subscriber**로 전달하기 위해 호출하는 함수

이 때 subscriber의 첫번째 요청이란 subscriber가 Subscription protocol에 정의된 request(_:)를 호출하는 행위를 의미합니다.

</br>

#### 3. receive(completion:)

> subscriber에게 이벤트 방출(publishing)이 정상적으로 끝났는지 또는 에러로 인해 끝났음을 알려줄때 호출되는 함수

이 때 파라미터 타입은 Subscribers.Completion

</br>
</br>

### Subscriber 만들기

> Combine은 Publisher 타입의 operato를 통해 아래와 같은 subscriber를 제공합니다.

- **sink(receiveCompletion:receiveValue:)** : 종료 시그널을 받거나 매번 새로운 요소를 받았을 때 임의적인 closure을 실행
- **assign(to: on:)** : 매번 새로 받은 값을 주어진 인스턴스의 key path로 정의되는 property에 할당
