## ✨ Property Observer didSet에 대해 알아보자 !
> 배열을 저장하는 변수와 배열의 길이를 저장하는 변수를 정의해 사용할 경우, 배열의 길이는 배열이 업데이트되고 난 후 자동으로 업데이트 되어야 하는 값이다. 이때, didSet을 사용하면 배열이 업데이트될 때 마다 길이를 따로 갱신해줄 필요가 없게 된다 !
> 사실 이게 didSet의 전부다 ! 그래도 조금 더 알아보자 !

</br>
</br>

### didSet이란? 

- didSet은 **프로퍼티의 값이 변경된 직후**에 호출되는 옵저버이다.
- 이때, 이전 프로퍼티의 값이 oldValue 로 제공된다.
- 프로퍼티 옵저버를 사용하기 위해서는 프로퍼티의 값이 반드시 `초기화` 되어 있어야 한다.
- 클래스의 `init()`안에서 값을 할당 할 때는 didSet, willSet은 호출되지 않는다.

 </br>
 </br>

```swift
@State var previousTravel: [Travel]? {
        didSet {
            travelCnt = Int(previousTravel?.count ?? 0)
        }
    }
```
</br>

위의 예시를 살펴보면 previousTravel이 갱신될 때마다 travelCnt이 해당 여행이 개수로 저장되는 것을 알 수 있다.
