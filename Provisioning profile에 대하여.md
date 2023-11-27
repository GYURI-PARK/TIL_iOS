## 협업을 위한 인증서와 프로비저닝 프로파일에 대해 알아보자 ! 
> 우선 문제상황을 설명하자면 팀 프로젝트를 나의 개발자 계정을 사용해 앱 스토어 배포를 해본 게 이번이 처음이었다.
> 개발자 계정에 팀원들의 apple id를 등록해 놓기만 하면 모든 팀원이 배포할 수 있을 줄 알았지만..역시 애플은 생각보다 만만치 않았고, 다른 블로그들을 보고 순서대로 따라 하기만 하면 문제는 해결되지만 `왜? 이렇게 해야 하는 거지?`라는
> 물음은 해결되지 않았다.
> 그래서 오늘 만난 여러 개념들을 하나하나 자세히 살펴볼 겸, 다신 까먹지 않기 위해 정리해 보려 한다. </br>
> 그러니깐 이번 포스팅은 1. 팀 프로젝트에서는 **왜** 프로비저닝 프로파일, 인증서가 필요한지  </br>
> 2. 하나의 개발자 계정을 팀이 사용하기 위해선 **무엇**이 필요한지 </br>
> 3. 그래서 **어떻게** 하면 되는지 </br>
> 에 대해 정리할 예정이다.

</br>
</br>

## 프로비저닝 프로파일(Provisioning profile)이란? 

Apple은 보안성이 매우 뛰어나다. 
기본적으로 **Apple 인증서로 서명되어 App Store에 올라간 앱**만이 iOS 기기에서 실행될 수 있다. </br>
그렇기 때문에 앱이 실행하기 위한 여러가지 제약조건들(`해당 앱은 어느 디바이스에서 실행될 수 있는지?`와 같은 것들)이 존재하는데 앱의 환경이 이러한 제약 조건과 일치할 경우에만 실제 디바이스에서 실행할 수 있다.

프로비저닝 파일은 iOS 디바이스와 Apple 인증서 사이의 비교를 위해 **앱을 실행하기 위한 규칙들을 하나의 파일에 모아놓은 파일**이다.
</br>


### 프로비저닝 프로파일 안에 들어 있는 것들
<img width="610" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/0aba4bc7-6e38-4e11-b349-a660933f6004">

- App ID (Bundle identifier)
- 인증서 (Development Certificate)
- 허용된 디바이스 (각 기기들의 UDID 목록)
- 권한 및 자격 (Entitlements)
 
와 같은 것들이 하나의 프로비저닝 프로파일 안에 포함되어 있다.
따라서 테스트하고 싶은 (개발에 사용되는) 모든 디바이스은 해당 프로비저닝 프로파일을 가지고 있어야 하며,
단일 장치에 여러 개의 프로비저닝 프로파일이 포함될 수도 있다.


</br>
</br>

## 💡 1. 이거 왜 필요한데?

Certificate, Provisioning Profile, CSR... 무시무시해보이는 이것들은 사실 XCode를 사용해 개발을 했다면 항상 쓰고 있었을 것이다. </br>
다만, 아래 사진과 같이 `Automatically manage signing`으로 선택되어, XCode가 자체적으로 프로비저닝 프로파일을 생성하고 등록하기 때문에 실기기에서 문제없이 실행되는 것이다. </br>

<img width="847" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/2d46b3c2-99a3-4ede-ae2b-13c60639d3bb">

</br>
</br>

이 때 Bundle identifier는 프로젝트에 대한 고유 식별자와 같은 것인데, 다른 사람들은 내가 사용하고 있는 Bundle Identifier를 사용하지 못하게 되고, 각 앱은 고유한 Bundle Id를 가져야한다. </br>

따라서 팀 내 개발자들과 공용으로 사용하기 위해선 프로비저닝 프로파일을 생성하고, 공유해 디바이스에서 실행 가능한 앱을 Apple에게 등록해 *해당 디바이스는 개발자 계정에 신뢰된 디바이스에요~*를 알려줄 필요가 있는 것이다. </br>

</br>

정리하면

- 팀 내에서 동일한 앱을 개발하고 테스트하기 위해서는 모든 개발자들이 동일한 프로비저닝 프로파일을 사용해야 한다.
- 즉, 프로비저닝 프로파일을 팀 내에서 공유해 모든 개발자가 동일한 서명 및 권한을 사용할 수 있도록 해야한다.

</br>
</br>

## 💡 2. 그렇다면 개발자 계정 주인은 무엇을 하면 되는가?
### 1) 팀 개발자 추가하기

해당 작업은 [App Store Connect](https://appstoreconnect.apple.com/access/users)에서 하면된다. </br>
App Store Connect 홈페이지의 `사용자 및 액세스` > `사용자`에서 팀 내 개발자들을 추가해주면 된다. </br>

<img width="686" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/ca9bfcae-c2c3-4a75-b369-eb4d0d03b804">

초대를 보내고 팀원이 초대를 수락하게 되면, </br>

<img width="953" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/e1fad6e2-9ba6-43e2-ba75-45f3181c9997">

팀 개발자가 나의 개발자 계정에 추가된 것을 확인할 수 있다. </br>

</br>

### 2) 앱 등록하기

해당 작업은 [Apple Developer 홈페이지](https://developer.apple.com/account/resources/certificates/list)에서 하면 된다. </br>

`계정` > `Certificates, Identifiers & Profiles` > `Identifiers`로 이동한 다음 아래의 그림에 나온 파란색 플러스 버튼을 눌러 **App Id**를 생성해준다.

<img width="1081" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/07a1dc6d-3820-4a01-acf7-97e692149950">
<img width="1112" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/66e8897d-9774-4147-b0a0-a083c56c7c7d">
<img width="1015" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/21d11ba1-d2a9-4179-86e0-6f32b349a25d">

위 사진들의 continue를 차례로 눌러주면 다음 화면을 만날 수 있다. 

<img width="1053" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/b8b34b7e-ae87-441a-9ffb-eb035df57c66">

- Description에 앱에 관한 간단한 설명을 적어준다.
- Bundle Id를 설정해준다.
- 이 때 정한 Bundle Id가 앱의 고유한 id가 된다.
- 밑에 Capabilities들 중 앱에서 사용하는 기능들을 체크해주면 된다. (추후 수정 가능)

<img width="204" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/ea31b0db-8693-4c29-b1c8-e5f9804100f4">

위 과정을 모두 거친 후 register을 누르면 Identifiers에 추가된 것을 볼 수 있을 것이다. </br>

</br>

### 3) Certificate Signing Request(CSR) 생성하기
