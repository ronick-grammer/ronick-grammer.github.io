---
title:  "깃허브 소스로 Azure Wep App 서버에 배포하기(실패)"
excerpt: "깃허브 소스로 Azure Wep App 서버에 배포하기(실패)"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - Azure
<!-- tags:
  - Azure 
last_modified_at: 2021-01-04 -->
---

### 결론적으로는 깃허브 소스를 Azure 를 이용해 서버에 배포하기는 '실패'하였다.
"정확히는 배포는 성공을 했는데 반영에는 실패하였다. 그리고 원인을 찾지 못하였다" 가 되겠다. 우선 깃허브 리포지토리의 스펙을 적자면 다음과 같다.<br>

- 프레임 워크: Spring
- 언어: 자바
- 빌드 도구: Gradle (중요)

그리고 배포하는 과정의 배경은 다음과 같다.<br><br>

1. Azure 웹사이트에서 App Service 형식의 리소스를 만든다.
2. 깃허브에 리포지토리를 만들어 스프링 프로젝트 소스를 커밋, 푸시한다.
3. 1번에서 만든 App Service의 배포 센터에 가서 배포 기본 소스를 깃허브로 설정한 후, 2번의 스프링 프로젝트 리포지토리와 연동한다.
4. Azure 는 3번에서 연동된 깃허브 소스를 서버에 배포한다. 그 후, 리포지토리에서 커밋이 발생할 때 마다 매번 배포를 수행한다.

위의 과정을 거치면 웹 서버 구축이 되어 만든 웹사이트를 볼 수가 있다. 순조로워 보이지만 여기에는 함정이 존재한다.<br>
Azure는 3번을 수행할 때 기본 소스를 깃허브로 설정을 하면 리포지토리의 루트 아래에 워크 플로우 파일(.yml)을 생성한다.<br>

- 워크 플로우 파일 위치: root/.github/workflows/{App Service 이름}.yml


워크 플로우 파일의 역할은 간단히 말해 Azure 가 항상 리포지토리의 작업 상황을 감시한다. 변경이 생기면 바로 빌드를 하고 배포하여 웹 서버에 반영을 할 수 있게 해주는 것이다.<br>
그런데 이 워크 플로우 파일에 설정된 빌드 도구는 Maven 이다. 다시 말해 만약 우리가 Gradle 빌드 도구를 사용해서 Spring 프로젝트를 만든 것이라면 배포도 전에 빌드 과정에서 실패를 한다.
바로 아래 처럼.<br>


<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/Azure/1.png" width="70%">
</p>
<p align="center">(수 많은 시도와 실패의 흔적..)</p><br><br>

빌드에 실패한 내용을 보도록 하자. 아래에 보이듯이 <span style="color:red">Build with Maven</span>에서 실패를 하였다. Gradle 프로젝트인데 Maven 으로 빌드를 시도하니
실패는 당연한 결과다.

<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/Azure/2.png" width="100%">
</p>
<br>

### 해결책을 찾아보자
무척 간단하다. Gradle 프로젝트를 그에 맞게 Gradle 로 빌드를 해주면 된다. 그 전에 참고로 내가 만든 Azure App Service 를 만들 때의 스택은 다음과 같다.

<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/Azure/3.png" width="60%">
</p>

<br>
이를 바탕으로 배포 기본 소스를 깃허브 리포지토리로 설정을 하면 아래내용(일부만 캡쳐)으로 워크 플로우 파일이 생성되고 핵심적으로 바꾸어야 할 부분은 빨간박스에 표시해 두었다.

<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/Azure/4.png" width="80%">
</p>
<br>

- 'name:' 같은 경우에는 빌드 과정에서 표시되는 이름이다. 변경하지 않아도 상관은 없지만 과정에 맞는 이름으로 변경이 되어야 적합하므로 변경하는 것을 권장한다.
- 'run:' 사용할 빌드 도구를 명시한다. 배포할 깃허브 소스가 Gradle 프로젝트라면 루트에 gradlew가 있을 것이고 이를 사용해서 빌드를 하도록 설정한다.
- 'path:' Gradle 은 빌드 후 생성된 .Jar , .War 같은 파일들을 'build/libs' 폴더 밑에 위치시킨다. 

<br>
그리고 나서 build.gradle 로 가서 plugin 안에 한 줄을 추가해준다. <span style="color:red">id: 'war'</span> <br><br>

<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/Azure/5.png" width="80%">
</p>
<br>

모든 설정을 완료하고 커밋 푸시를 하면 새로 빌드를 시작하고 배포를 한다.

<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/Azure/6.png" width="100%">
</p>
<p align="center">(성공!!! 그러나..)</p>

### 배포는 되었으나 반영이 안된다.
이유가 무엇인지 며칠을 검색해 보았지만 찾지 못하였다.. 캐시도 삭제해보고 별걸 다해봤으나 소용이 없었다. 그래서 다른 방법을 쓰기로 했다. <br>
바로 배포 기본 소스를 설정하는 것이 아니라 Azure CLI(Command Line Interface)를 이용해서 로컬에서 배포를 해서 반영하는 것이다.
디음 포스트에 쓰기로 한다..



