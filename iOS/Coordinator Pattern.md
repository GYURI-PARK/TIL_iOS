## ✨ Coordinator Pattern
> 두 달 동안 허슬러즈에서 터닝이라는 앱을 만들며 공부하기로 했다. 어제 처음 코드리뷰 세션이 있었는데, 처음 들어본 개념, 한 번도 안 써본 기술들이 많았다. 하나하나 차근차근 열심히 공부해봐야겠다. ㅍ ㅏ ㅇ ㅣ 팅..

</br>
</br>


### 💡 왜 필요할까 ?

한 마디로 정리하면, 기존의 ViewController에서 처리하는 화면 전환을 수행함으로써, ViewController간의 결합도를 낮춰주고, 유지보수를 편리하게 하기 위함이다. </br>

<img width="277" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/b4a3626a-1750-4e14-b2ee-bc4aef09cba0">

</br>

기존 iOS에서는 UINavigationController를 사용해 Stack 방식으로 화면 전환을 처리한다. </br>
새로운 화면은 Push하고, 이전 화면은 Pop을 사용해 돌아가게 된다.  </br>

</br>

이러한 화면 전환 코드는 ViewController 내부에 작성되게 되고, 화면 전환을 위해 부모 ViewController는 자식 ViewController를 알아야하고,
자식 ViewController 또한 부모 ViewController 내부에서 인스턴스를 생성해야 하기 때문에 ViewController 간의 결합도가 높아질 수 밖에 없게 된다.

</br>
</br>

### 💡 Coordinator Pattern이란 ?

Coordinator 패턴에서는 위의 화면 전환 역할을 ViewController가 아닌 Coordinator가 담당하게 된다. </br>
즉, ViewController로 부터 화면 전환의 부담을 줄여주고, 화면 전환을 보다 더 관리하기 쉽도록 도와주기 위한 패턴인 것이다. </br>

<img width="915" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/0bbf3c0d-4df2-4b1a-997d-50d4b7a58ff2">

각 ViewController는 이전에 어떤 컨트롤러가 있었는지, 다음에 어떤 컨트롤러가 오는지 알 필요가 없다. </br>
화면 전환 flow는 coordinator가 관리한다. </br>

</br>

### 💡 Coordinator 역할

위에서 설명한 Coordinator의 역할을 정리하면 다음과 같다. </br>

1. 자신이 담당하는 ViewController의 필요한 인스턴스를 생성해 주입한다.
2. 의존성이 주입된 ViweController 인스턴스를 화면에 표시한다.

</br>
</br>


### 💡 구현하기

#### 1. Coordinator 프로토콜 생성

```swift
protocol Coordinator: AnyObject {
  var navigationController: UINavigationController { get }
  var parentCoordinator: Coordinator? { get set }
  var childCoordinators: [Coordinator] { get set }
}

extension Coordinator {
  func removeChildCoordinator(child: Coordinator) {
    childCoordinators.removeAll { $0 === child }
  }
}
```

> 화면 전환을 위한 Coordinator를 생성하고, 필요한 작업들을 extension으로 구현한다. </br


#### 2. 해당 프로토콜을 채택하는 객체 생성

Coordinator Pattern의 구조를 더 자세히 나타내면 다음과 같다. </br>

<img width="639" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/7e4e094e-dae7-45b6-8166-bfe9d799d7ff">


**AppCoordinator** </br>

Coordinator 패턴의 최상단에는 AppCoordinator가 존재한다. </br>

```swift
final class AppCoordinator: Coordinator {
  let navigationController: UINavigationController
  weak var parentCoordinator: Coordinator?
  var childCoordinators: [Coordinator] = []

  init(navigationController: UINavigationController) {
    self.navigationController = navigationController
  }

  func start() {
    let mainCoordinator = MainCoordinator(navigationController: navigationController)

    mainCoordinator.parentCoordinator = self
    childCoordinator.append(mainCoordinator)
    mainCoordinator.start()
  }
}
```

</br>

AppCoordinator는 start 메서드 내부에서 root ViewController로 보일 화면의 Coordinator의 start 메서드를 호출해준다. </br>
이때 start 메서드는 `AppDelegate` 또는, `SceneDelegate`에서 호출되게 된다.

</br>

**MainCoordinator**

```swift
final class MainCoordinator: Coordinator {
  let navigationController: UINavigationController
  weak var parentCoordinator: Coordinator?
  var childCoordinators: [Coordinator] = []

  init(navigationController: UINavigationController) {
    self.navigationController = navigationController
  }

  func start() {
    let viewModel = MainViewModel()
    let mainVC = MainViewController(viewModel: viewModel)
    mainVC.coordinator = self
    navigationController.pushViewController(mainVC, animated: true)
  }

  func showSearchView() {
    let searchCoordinator = SearchCoordinator(navigationController: navigationController)
    childCoordinator.append(searchCoordinator)
    searchCoordinator.parentCoordinator = self
    searchCoordinator.start()
  }
}
```
> MainCoordinator 내부에는 start 메서드와 다음 자식 화면으로 이동하는 메서드를 구현해준다. </br>

</br>

**SearchCoordinator**

```swift
final class SearchCoordinator: Coordinator {
  let navigationController: UINavigationController
  weak var parentCoordinator: Coordinator?
  var childCoordinators: [Coordinator] = []

  init(navigationController: UINavigationController) {
    self.navigationController = navigationController
  }

  func start(book: Book) {
    let viewModel = SearchViewModel()
    let searchVC = SearchViewController(viewModel: viewModel)
    searchVC.coordinator = self
    navigationController.pushViewController(searchVC, animated: true)
  }

  func removeCoordinator() {
    parentCoordinator?.removeChildCoordinator(child: self)
  }
}
```
> 모든 Coordinator가 동일하게 start 메서드가 구현되어 있고, 화면이 사라지면, 해당 Coordinator도 같이 사라질 수 있도록 removeCoordinator 메서드를 구현해준다.





