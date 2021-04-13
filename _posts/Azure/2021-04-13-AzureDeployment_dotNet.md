---
title:  "깃허브 소스 Azure Wep App 서버에 배포하기 (feat .Net Core)"
excerpt: "깃허브 소스 Azure Wep App 서버에 배포하기 (feat .Net Core)"
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

### .Net 기반 웹사이트가 갑자기 서비스가 중단이 되다.
갑자기 잘만 작동하던 웹 서버가 먹통이 되었다. 서둘러서 소스코드를 점검하고 배포를 하는 순간 '빌드 실패' 응?
깃허브 리포지토리를 Azure 웹 서버에 배포를 하고 나서 코드를 일절 변경한 적이 없는데 다시 배포를 하니 실패라고 한다.
Action 탭에 들어가 빌드(Build)와 배포(Deploy)의 워크플로우를 살펴 보았다.


<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/Azure/deploy Error01.png" width="100%">
</p>
<p align="center">(예전에는 안뜨던 에러가 뜬다)</p><br>

위에서 보이듯이 총 두 개의 Warning 과 세 개의 Error 가 발생했다. 우선 에러 내용 부터 살펴 보도록 하자.

- 42행: Microsoft.EntityFrameworkCore.Tools.Dotnet 을 찾을 수가 없다고 한다.
- 43행: Microsoft.VisualStudio.Web.CodeGeneration.Tools 를 찾을 수 없다고 한다.
- 44행: 파라미너 'path'의 값이 null 일 수 없다고 한다.

위 세 개의 에러는 루트에 위치한 DotnetCoreServer.csproj 에서 발생하였다. 이 파일은 Gradle 로 Spring 프로젝트를 생성을 해보았다면 build.gradle 처럼 라이브러리와 빌드 설정을 
해주는 파일이라고 보면 된다. 하지만 사실 위의 Error 들을 해결하려면 Warning 을 해결하면 된다. 2.1 이상의 버전부터는 별도로 위의 패키지(혹은 레퍼런스)를 따로 설치할 필요없이 자동으로 
해준다. <br><br>

즉, .net core 2.0 버전을 업데이트 해주면 된다. 나 같은 경우에는 2.1 로 업데이트 해주었다. 한번에 최신 버전으로 업데이트(6.0 이상) 해줄 수 있겠지만 시간상 나중에 하기로 하였다.
<br>

``` xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<Project Sdk="Microsoft.NET.Sdk.Web">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <!-- <TargetFramework>netcoreapp2.0</TargetFramework> -->
    <TargetFramework>netcoreapp2.1</TargetFramework>  <!-- 추가 -->
    <RuntimeIdentifiers>win10-x64;osx.10.11-x64</RuntimeIdentifiers>
  </PropertyGroup>
  <ItemGroup>
    <Folder Include="wwwroot\"/>
  </ItemGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.0.0"/>
    <PackageReference Include="Microsoft.AspNetCore" Version="2.0.0"/>
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="2.0.0"/>
    <PackageReference Include="Microsoft.Extensions.Logging.Debug" Version="2.0.0"/>
    <PackageReference Include="MySqlConnector" Version="0.22.0"/>
  </ItemGroup>
  <!--
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="1.0.0"/>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="1.0.0" />  
  </ItemGroup>
-->
</Project>

```
<br>
그리고 여기서 잠깐!! 한가지 더 확인 해주어야 할 것이 있다. Azure 에서 배포 소스를 깃허브 소스로 설정할 때에 빌드 런타임 스택과 업데이트 할 .net core의 버전을 호환시켜야 한다.
무슨 소리냐 하면 아래 사진처럼 해주어야 한다는 것이다.<br><br>

<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/Azure/deploy build runtime stack setting.png" width="100%">
</p>
<p align="center">(이렇게 하고 저장)</p>


빌드와 배포에 성공하였다.<br>

<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/Azure/build and deploy success.png" width="100%">
</p>

