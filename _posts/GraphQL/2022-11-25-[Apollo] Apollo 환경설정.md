---
title:  "[Apollo] Apollo iOS 환경설정"
excerpt: "[Apollo] Apollo iOS 환경설정"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - GraphQL
  - iOS
  - Swift
---

### Apollo란?
Apollo란 애플리케이션을 위한 오픈소스 GraphQL 클라이언트이다. Apollo는 GraphQL 서버간의 쿼리 실행과 변환을 가능하게 해주고, 결과값을 현재 사용하는 언어의 체제에 맞는 타입으로 반환한다.

### Apollo iOS SDK 설치하기

아래와 같이 Xcode의 Package Manager에서 Apollo SDK 를 설치한다. <br>
Package URL: https://github.com/apollographql/apollo-ios

![InstallApollo_1](https://user-images.githubusercontent.com/73280175/160070808-9e1ec78a-4c54-4890-99d3-a7590a809c93.png)

### 코드 생성을위해 필요한 것

- GraphQL Schema(스키마): 서버에 있는 쿼리와 데이터의 타입을 모아둔 리스트들을 담고 있다.
- operation: 스키마에 기반한 데이터를 요청하는 타입(query, mutation 등이 있다)이다.

### 서버로 부터 Schema 가져오기
Apollo iOS SDK는 서버가 제공하는 GraphQL Schema의 로컬 복사본이 필요하다. 
Apollo가 튜토리얼에서 공식적으로 제공하고 있는 서버로 부터 Schema를 가져온다. 이를 위해 Xcode의 "Run Script Build Phase" 를 추가하여 Apollo CLI를 사용한다.

<br>
<img width="868" alt="build_phases" src="https://user-images.githubusercontent.com/73280175/160075240-99575dbc-1b07-4dcb-936f-c0cc1a7c5a35.png">
<br>
<img width="269" alt="new_run_script_phase" src="https://user-images.githubusercontent.com/73280175/160075174-8efda251-5c23-477a-8ee2-c0f5ff5b4f98.png">
<br>
<img width="702" alt="rename_run_script" src="https://user-images.githubusercontent.com/73280175/160075175-24a8bde1-9b18-49ad-beb6-44bd806aa1b8.png">

그리고 Apollo phase를 클릭 확장하여 아래 스크립트를 붙여 넣는다.

```sh
# Don't run this during index builds
if [ $ACTION = "indexbuild" ]; then exit 0; fi

# Go to the build root and search up the chain to find the Derived Data Path where the source packages are checked out.
DERIVED_DATA_CANDIDATE="${BUILD_ROOT}"

while ! [ -d "${DERIVED_DATA_CANDIDATE}/SourcePackages" ]; do
  if [ "${DERIVED_DATA_CANDIDATE}" = / ]; then
    echo >&2 "error: Unable to locate SourcePackages directory from BUILD_ROOT: '${BUILD_ROOT}'"
    exit 1
  fi

  DERIVED_DATA_CANDIDATE="$(dirname "${DERIVED_DATA_CANDIDATE}")"
done

# Grab a reference to the directory where scripts are checked out
SCRIPT_PATH="${DERIVED_DATA_CANDIDATE}/SourcePackages/checkouts/apollo-ios/scripts"

if [ -z "${SCRIPT_PATH}" ]; then
    echo >&2 "error: Couldn't find the CLI script in your checked out SPM packages; make sure to add the framework to your project."
    exit 1
fi

cd "${SRCROOT}/${TARGET_NAME}"
# "${SCRIPT_PATH}"/run-bundled-codegen.sh codegen:generate --target=swift --includes=./**/*.graphql --localSchemaFile="schema.json" API.swift
"${SCRIPT_PATH}"/run-bundled-codegen.sh schema:download --endpoint="https://apollo-fullstack-tutorial.herokuapp.com/graphql"
```

위 스크립트를 붙여넣고 빌드를 누르면 프로젝트 root 디렉토리에 schema.graphqls(혹은 schema.json)이 생성된다. <br>
디렉토리에는 생성되었지만, .xcodeproj 에는 적용이 안되었으므로 해당 파일을 직접 추가해준다. <br>
schema 파일은 코드를 생성하는 용도에만 사용되므로 모든 target을 체크해제시켜 불필요한 application bundle 사이즈를 늘리지 않도록 한다.

### operation-qeury 
schema를 서버로 부터 가져왔으니 이제 query를 생성해서 이에 맞는 swift 코드를 생성하자.

첫째, 아래와 같이 필요한 데이터만 가져오도록 ```.graphql``` 파일에 쿼리를 생성한다.

```sql

query ExampleQuery {
  launches {
    cursor
    hasMore
    launches {
      id
      site
    }
  }
}

```

<br>
둘째, 그리고 위의 과정에서 붙여넣었던 Apollo build phase에다가 아래 스크립트를 붙여넣는다. <br>

```sh
# Don't run this during index builds
if [ $ACTION = "indexbuild" ]; then exit 0; fi

# Go to the build root and search up the chain to find the Derived Data Path where the source packages are checked out.
DERIVED_DATA_CANDIDATE="${BUILD_ROOT}"

while ! [ -d "${DERIVED_DATA_CANDIDATE}/SourcePackages" ]; do
  if [ "${DERIVED_DATA_CANDIDATE}" = / ]; then
    echo >&2 "error: Unable to locate SourcePackages directory from BUILD_ROOT: '${BUILD_ROOT}'"
    exit 1
  fi

  DERIVED_DATA_CANDIDATE="$(dirname "${DERIVED_DATA_CANDIDATE}")"
done

# Grab a reference to the directory where scripts are checked out
SCRIPT_PATH="${DERIVED_DATA_CANDIDATE}/SourcePackages/checkouts/apollo-ios/scripts"

if [ -z "${SCRIPT_PATH}" ]; then
    echo >&2 "error: Couldn't find the CLI script in your checked out SPM packages; make sure to add the framework to your project."
    exit 1
fi

cd "${SRCROOT}/${TARGET_NAME}"
"${SCRIPT_PATH}"/run-bundled-codegen.sh codegen:generate --target=swift --includes=./**/*.graphql --localSchemaFile="schema.json" API.swift
# "${SCRIPT_PATH}"/run-bundled-codegen.sh schema:download --endpoint="https://apollo-fullstack-tutorial.herokuapp.com/graphql"

```

<br>
셋째, 빌드를 하여 루트 디렉토리에 ```API.swift``` 파일이 생성한다.


위의 API.swift 파일을 살펴보면 위에서 생성했던 쿼리의 내용에 맞게 구조체가 구현되어 있는 것을 볼 수 있다. 쿼리문의 내용을 변경하고 빌드를 하면 그 즉시 ```API.swift```의 내용이 이에 맞게 반영한다.

### 쿼리 테스트하기
위에서 생성한 API.swift안의 operation을 사용하기 위해서는 ```ApolloClient``` 인스턴스를 생성해야한다. <br>
ApolloClient 는 위에서 생성한 API.swift내의 operation 코드를 사용하여 서버와 네트워크 통신을 진행한다. <br>
그리고 ApolloClient는 싱글톤으로 생성되는 것이 권장된다.

첫째, ```Network.swift``` 만들고 아래의 코드를 붙여넣는다.

```swift
import Foundation
import Apollo

class Network {
  static let shared = Network()

  private(set) lazy var apollo = ApolloClient(url: URL(string: "graphQL 스키마를 가지고 있는 서버 url을 넣으세요")!)
}

```
<br>
둘째, ApolloClient 인스턴스가 서버와 정확한 통신을 하고 있다는 것을 테스트하기 위해서 AppDelegate.swift의 ```application:didFinishLaunchingWithOptions``` 메서드 안에 ```return```문 위에 아래 코드를 붙여넣는다.

```swift
Network.shared.apollo.fetch(query: LaunchListQuery()) { result in
  switch result {
  case .success(let graphQLResult):
    print("Success! Result: \(graphQLResult)")
  case .failure(let error):
    print("Failure! Error: \(error)")
  }
}
```

<br>
셋째, 애플리케이션을 빌드/실행 시켜 응답값을 콘솔에서 확인한다.
















