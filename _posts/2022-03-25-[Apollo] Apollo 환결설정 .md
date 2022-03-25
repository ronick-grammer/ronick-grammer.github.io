---
title:  "[Apollo] Apollo 환결설정"
excerpt: "[Apollo] Apollo 환결설정"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - Apollo
  - GraphQL
  - IOS
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
- operation: 스키마에 기반한 데이터를 요청할 쿼리이다.

### 서버로 부터 Schema 가져오기
Apollo iOS SDK는 서버가 제공하는 GraphQL Schema의 로컬 복사본이 필요하다. 

1. Apollo가 튜토리얼에서 공식적으로 제공하고 있는 서버로 부터 Schema를 가져온다. 이를 위해 Xcode의 "Run Script Build Phase" 를 추가하여 Apollo CLI를 사용한다.
<img width="868" alt="build_phases" src="https://user-images.githubusercontent.com/73280175/160075240-99575dbc-1b07-4dcb-936f-c0cc1a7c5a35.png">
<br>
<img width="269" alt="new_run_script_phase" src="https://user-images.githubusercontent.com/73280175/160075174-8efda251-5c23-477a-8ee2-c0f5ff5b4f98.png">
<br>
<img width="702" alt="rename_run_script" src="https://user-images.githubusercontent.com/73280175/160075175-24a8bde1-9b18-49ad-beb6-44bd806aa1b8.png">

그리고 Apollo phase를 클릭 확장하여 아래 스크립트를 붙여 넣는다.

<details><summary>스크립트</summary>
    
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
    
</details>

위 스크립트를 붙여넣고 빌드를 누르면 프로젝트 root 디렉토리에 schema.graphqls(혹은 schema.json)이 생성된다. <br>
디렉토리에는 생성되었지만, .xcodeproj 에는 적용이 안되었으므로 해당 파일을 직접 추가해준다. <br>
schema 파일은 코드를 생성하는 용도에만 사용되므로 모든 target을 체크해제시켜 불필요한 application bundle 사이즈를 늘리지 않도록 한다.

