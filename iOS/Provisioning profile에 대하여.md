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
> CSR은 인증서를 발급받기 위한 일종의 요청서라고 보면 된다.

</br>

CSR는 맥북의 `키체인 접근 앱`에서 만들 수 있다. </br>

<img width="601" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/3a4c8697-4174-4ffc-b5fa-082eed6456f3">

`키체인 접근` > `인증서 지원` > `인증 기관에서 인증서 요청`을 차례로 클릭해준다. </br>
</br>

<img width="616" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/7dd2de0c-3bb3-4a01-8cc9-a23a6e0430cd">

> 인증서 정보를 입력하는 창을 만나게 되는데 이때, 이메일 주소는 꼭 Apple 계정의 이메일 주소가 아니어도 된다.

본인의 이메일을 입력한 후, `디스크에 저장됨`을 체크해주면 Finder에서 아래와 같은 파일이 만들어진 것을 확인할 수 있을 것이다.

<img width="336" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/e5032751-5969-4c87-b4b2-852566abc7f5">

</br>
</br>

### 4) 인증서(Certificates) 생성하기
> 인증서는 Xcode에서도 만들 수 있고, Apple Developer 사이트에서도 직접 생성이 가능하다.

#### 1. Xcode에서 생성하기
<img width="255" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/47d7eff2-0e35-4914-a9b0-6c84fca1fbfe">
 
> `Xcode` > `Settings`에 들어간다.

<img width="694" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/1a528cbf-df0f-46ff-a0ec-bcc9796bc4aa">

> `Accounts` 탭으로 이동 후 `Manage Certificates`를 클릭한다.

<img width="214" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/c39567c7-3f4d-40bb-a1eb-0912bb62bddf">

> 플러스 버튼을 누르면 다음과 같은 목록을 볼 수 있다.

- 이 때 Apple Development(개발용), Apple Distribution(배포용) 인증서를 각각 만들어 준다.
- 만들어진 인증서는 Apple Developer 사이트에서 확인할 수 있다.

<img width="934" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/a67d09ff-6a1b-4182-a234-0675fe5a2564">
> 애플 개발자 계정의 Certificates에 자동으로 만들어진 것을 확인할 수 있다.

</br>
</br>

#### 2. 사이트에서 생성하기
<img width="598" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/ae391c3a-b7bc-4f43-89c5-fcc4a7874632">

> Apple Developer 사이트에서 플러스 버튼을 클릭해 직접 만들어 줄 수도 있다. 

<img width="629" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/6219282d-f358-43ac-8853-a7b66ef56d2c">

> 목적에 맞게 선택해주면 된다.

- 여기서도 마찬가지로 Apple Development는 개발용, Apple Distribution은 배포용이다.
- 각각 만들어주면 된다.

<img width="1045" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/80a3bc26-ddda-4e2c-bb16-2eee30b1afb6">

> 앞서 만든 CSR 파일을 업로드 한다.

위 과정을 모두 거치고 나면, 새로운 인증서가 만들어진 것을 확인할 수 있다.
둘 중 한 가지 방법을 선택해 인증서를 만들고, 해당 인증서를 클릭해 들어가게 되면 아래와 같이 `Download` 받을 수 있을 것이다.

<img width="900" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/2fb22b31-0fb4-49cb-a42c-797a0ba66e11">
</br>

다운로드 받은 파일은 `.cer`형식의 파일로 아래와 같이 다운로드 항목에서 확인할 수 있다. 
<img width="709" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/579fef3b-8c9d-459b-b1aa-4718459441d4">
</br>

만들어진 인증서는 키체인에 등록해주어야한다. </br>
방법은 간단하게 더블클릭만 해주면 키체인이 등록되고, 맥북 키체인 접근 앱에서 확인할 수 있다.

<img width="748" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/57306243-2ba8-4163-ac6e-67e36be99ce7">

> 내 인증서 탭에서 확인해보면, 인증서와 해당 키가 만들어진 것을 확인할 수 있다.

만들어진 인증서와 개인키 모두 팀원들에게 공유해주어야 한다. </br>
인증서를 우클릭 후 `내보내기`를 선택한다. </br>
내보내기를 한 곳으로 이동해 만들어진 p12 파일을 클릭하면 다음과 같이 암호를 설정하도록 되어있다. </br>

<img width="489" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/e8b2a13b-8b3a-4068-b881-8f5c2e79a83a">

> 해당 암호는 팀원들이 파일을 접근할 때에도 필요하므로, 팀 내 개발자들 간의 공유가 필요하다. 

<img width="103" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/949e6f9b-2421-4553-8a03-d3eacfc4444a">

> 암호를 생성해주고 나면 p12 확장자를 가진 파일이 생성된 것을 확인할 수 있을 것이다.

</br>

배포용 인증서에 대해서도 똑같은 과정을 반복해주어 p12 확장자의 파일을 만들어 주어야한다. </br>
그렇게 만들어진 두 개의 파일(개발용, 배포용)은 카톡이나, 에어드랍, 슬랙 .. 등등을 이용해 팀원들에게 공유해주면 된다. </br>

</br>
</br>

### 5) 프로비저닝 파일 생성하기

프로비저닝 파일 생성 시 주의해야할 점은, 앱 안에 여러 타겟이 있는 경우, **각 타겟에 대한 Bundle ID와 프로비저닝 파일을 각각 생성**해주어야 한다. </br>
예를 들어 iOS와 watchOS를 모두 개발하고 있다면 iOS에 대한 bundle ID, watchOS에 대한 bundle Id를 각각 만들어주어야 한다는 것이다. </br>

<img width="1089" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/49f9d34c-325b-44a9-80a3-c9daca233b60">

> 프로비저닝 파일은 Apple Developer 사이트에 Profiles에서 만들 수 있다.

<img width="1033" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/d499a7b7-9e0f-494f-821f-196b8b0f1518">

> 먼저 개발용 프로비저닝 파일을 만들어 준다. 

<img width="562" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/4fceb0b8-1b8f-4d2f-8963-e5358a3e1389">

> 앞에서 만들어 둔 App Id를 선택해준다.

<img width="614" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/734503e2-2faa-48db-b13e-3e28f128febc">

> 개발자 인증서를 선택해준다.

이 때 하나라도 제대로 없다면 오류가 날 수 있기 때문에 무엇을 선택할지 잘 모르겠다면 **select all**을 선택해준다. </br>

<img width="1020" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/70b9f5c5-536b-4463-b6e1-a4ff8f2e18ba">

> 프로비저닝 파일 안에 포함시킬 디바이스를 등록해준다.

이 때 팀원들의 실기기(macBook, iPhone 등)를 모두 선택해주면 된다. </br>
팀원들의 기기는 `Devices`탭에서 추가할 수 있다.

</br>

<img width="541" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/f9965fc7-e8a0-4f9f-98db-605e98e04edb">

> 프로비저닝 파일의 이름을 설정해준다.

개발용과 배포용 두가지가 존재하기 때문에 프로비저닝 파일의 이름을 구분해주는 것이 좋다. </br>
Generate시 개발용 프로비저닝 파일이 만들어지게 된다. </br>

<img width="728" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/9bc7f6f1-ebc4-4c30-a5d0-db22d24d4155">

> 배포용도 같은 방식으로 만들어주면 된다. 

</br>

만들어진 프로비저닝 파일을 download하면 `.mobileprovision`형식의 파일이 생성될 것이다.

</br>

### 6) 파일 공유하기

앞서 만든 여러가지 파일들 중, `인증서(.p12)`와 `프로비저닝 파일(.mobileprovision)`을 팀원들에게 공유해주면 된다.

</br>
</br>

## 💡 3. 다른 팀원들이 해야할 일

계정 주인으로부터 받은 파일을 로컬에 저장하고, 인증서의 암호를 풀어주면 키체인에 등록되게 된다.
</br>

<img width="863" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/d07fe996-ca53-40d2-880b-189662173e6b">

</br>

Xcode에서 기존에 `Automatically manage signing`의 체크를 해제하고 저장한 프로비저닝 파일을 `import`해주면 팀원들 모두 같은 Bundle identifier를 사용해 개발할 수 있을 것이다.
