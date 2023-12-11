## ✨ removeLast() vs popLast() 뭐가 다를까?! 

> **배열의 마지막 요소를 없애고 없앤 값을 리턴**하는 두 가지 함수 removeLast()와 popLast()의 차이점에 대해 알아보자 !

</br>
</br>

우선 공식 문서에서 정의하고 있는 개념은 아래와 같다. </br>
<img width="832" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/19b4e905-5a61-4c8e-a0cc-549183a77d8a">
<img width="833" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/a59c8ccd-2cd8-4348-b115-566fb4bf938c">

두 함수 모두 **배열의 마지막 요소(element)를 없애고 그 값을 리턴**한다(*Removes and returns the last element of the collection.*)는 점은 동일하다. </br>

하지만 Return value를 보면 차이를 알 수 있다. </br>

</br>

### 1. removeLast()

<img width="817" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/180996b6-4260-44bd-97f2-af799e5554d2">

> removeLast()

</br>

removeLast()는 **값이 무조건 존재**해야하며 빈 배열에선 사용할 수 없다.

<img width="851" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/becd46ce-5cd5-4949-819b-27f16503bdef">

> 빈 배열에서 removeLast()를 사용할 경우 에러가 나게된다.


</br>
</br>

### 2. popLast()

<img width="829" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/1156c49d-adf2-4c23-9bc3-c187fe9bd1bb">

> popLast()

</br>

popLast()는 **값이 없을 경우 nil을 반환**한다.

```swift
var stack = ["스타벅스", "투썸"]

print(stack.popLast())
print(stack.popLast())
print(stack.popLast())
```

다음과 같은 식에서 각각의 출력 값은 아래와 같다. </br>

<img width="549" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/fee49c1d-7e92-415c-8f21-77d55a423ebf">

> 출력 값이 Optional이므로 popLast()를 사용할 경우 옵셔널 언래핑을 해주어야 한다 !
