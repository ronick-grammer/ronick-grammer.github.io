---
title:  "[Git]Branch(브랜치)"
excerpt: "[Git]Branch(브랜치)"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - Git
<!-- tags:
  - Database 
last_modified_at: 2021-01-04 -->
---
# 브랜치(Branch) 
여러 개발자들이 동시에 다양한 작업을 할 수 있게 만들어 주는 기능, 개념 혹은 작업영역이다.
각자 독립적인 작업 영역(저장소)안에서 마음대로 소스코드를 변경할 수 있으며 이렇게 분리된 작업 영역에서 변경된 내용은 나중에 원래의 버전과 비교해서 새로운 버전을 만들어 낼 수 있다.

```git
브랜치 만들기     : $git branch <branch name>
브랜치 목록보기    : $git branch 
브랜치 전환하기    : $git checkout <branch name>
브랜치 삭제하기    : $git banch -d <branch name>
```

## 실사용 예시
원격 저장소에 있는 브랜치를 기초로 새로운 브랜치를 만들어 변경사항을 커밋과 푸시를 해보자.<br><br>

원격 저장소: https://github.com/learn/programmer <br>
브랜치    : main(default) -> class/#1

```git
$git remote add origin http://github.com/learn/programmer  // 해당 원격 저장소의 이름을 'origin' 이라는 이름을 붙여서 원격으로 사용하겠다.
$git checkout main                                         // main 브랜치로 이동,
$git checkout class/#1                                     // class/#1 브랜치로 이동
$git checkout -b class/#1_Ronick                           // class/#1_Ronick 이라는 브랜치를 생성하고 동시에 이동
$git commit -a -m "commit"                                 // 현재 class/#1_Ronick 브랜치에 있으므로 "commit'이라는 메시지를 커밋하여 변경사항을 저장
$git push origin class/#1_Ronick                           // 위에서 생성한 원격 저장소 'origin'의 class/#1_Ronick 브랜치에 변경된 코드 사항들을 업로드
```

### 참고로,
* 원격 저장소의 브랜치를 못찾았을 경우 '$git remote update' 를 실행한다. 원격 저장소에서 새로운 브랜치가 올라왔는데 못찾는 경우 그 정보를 업데이트 해줘야 한다.
* 원격 저장소의 브랜치를 단순 checkout 하여 참고하면 cmmmit, push 할 수 없다.
