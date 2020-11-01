# learn git branch 메인파트
1번~3번

## 0. Introduction to Git Commits
- Repository : 하나의 프로젝트를 관리하기 위한 저장소
  - 프로젝트에 필요한 다양한 형식의 데이터를 저장할 수 있음
- Commit : 저장소의 스냅샷을 만들고 저장하는 것
  - 이 방법으로 수정사항에 대한 것들을 추적할 수 있음

# 실습
- 링크 : https://learngitbranching.js.org/ 

## 1.1 Git commit
```
git commit
git commit
```

## 1.2  Git 브랜치
- 브랜치를 많이 만들어도 메모리나 디스크 공간에 부담이 되지 않기 때문에, 브랜치를 통으로 만들기 보다는, 작은 단위로 잘게 나누는 것이 좋다고 함
- 브랜치 :  "하나의 커밋과 그 부모 커밋들을 포함하는 작업 내역"

```
git branch bugFix // bugFix 브랜치 만듬
git checkout bugFix // bugFix 브랜치로 이동
``` 

## 1.3 Git merge
Git의 합치기(merge)는 두 개의 부모(parent)를 가리키는 특별한 커밋을 만들어 낸다. 두개의 부모가 있는 커밋이라는 것은 "한 부모의 모든 작업내역과 나머지 부모의 모든 작업, 그리고 그 두 부모의 모든 부모들의 작업내역을 포함한다"라는 의미가 있다.
```
git branch bugFix // bugFix 브랜치 만듬
git chekcout bugFix // bugFix 브랜치로 이동
git commit // bugFix에서 한번 커밋
git checkout master // master 브랜치로 돌아감
git commit // master에서 한번 커밋
// 그러면 양쪽으로 갈라지는 형태가 나옴
git merge bugFix // bugFix 브랜치를 master에 합침
```

## 1.4 Rebase Introduction
- Rebase :  기본적으로 커밋들을 모아서 복사한 뒤, 다른 곳에 떨궈 놓는 것
- 리베이스를 하면 커밋들의 흐름을 보기 좋게 한 줄로 만들 수 있다는 장점이 있다. 리베이스를 쓰면 저장소의 커밋 로그와 이력이 깨끗해짐

```
git checkout bugFix    
git commit    
git checkout master    
git commit    
git checkout bugFix  // bugFix를 다시 선택
git rebase master // master에 rebase
```
- 이런 상태가 됨! C0 - C1 - C3(master) - C2(bugFix*) 
- 이후에 master 위치 바꿔주려면 아래와 같이 하면 됨
```
git checkout master
git reabase bugFix
```


## 2.1 Git에서 여기저기로 옮겨다니기
- HEAD :  현재 체크아웃된 커밋(현재 작업중인 커밋)
  - 항상 작업트리의 가장 최근 커밋을 가리킴 
  - 작업트리에 변화를 주는 git 명령어들은 대부분 HEAD를 변경하는것으로 시작
  - 일반적으로 HEAD는 브랜치의 이름(ex bugFix). 커밋을 하게 되면, bugFix의 상태가 바뀌고 이 변경은 HEAD를 통해서 확인이 가능
  - git checkout C2 하면 보이지 않던 HEAD가 보임
- HEAD 분리하기
  - HEAD를 분리한다는 것은 HEAD를 브랜치 대신 커밋에 붙이는 것을 의미함. 
  - EX) HEAD -> master -> C1 상태에서 git checkout C1하면 HEAD -> C1 상태가 됨!


```
git checkout C4 //  HEAD를 bugfix에서 분리하고 그 커밋에 붙임
```

## 2.2 Relative Refs (상대참조)
- Git에서 여기저기 이동할 때 커밋의 해시를 사용하는 방법은 귀찮다. 왜냐면 Git을 사용할 때 터미널화면 옆에 예쁘장하게 커밋트리가 보이진 않으니까... 매번 해시를 확인하려고 git log 명령어를 쳐야할 것임 
- 게다가 실제 Git에서는 해시들이 훨씬 더 김. Ex) fed2da64c0efc5293610bdd892f82a58e8cbc5d8

- 다행히도, Git에서는 해시가 커밋의 고유한 값임을 보여줄 수 있을 만큼만 명시해주면 됨
- fed2만 입력해도 됨!

- 상대 참조로 우리가 기억할 만한 지점(브랜치 bugFix라던가 HEAD라던가)에서 출발해서 이동하여 다른 지점에 도달해 작업을 할 수 있다.
- ### 방법
- 방법1) 한번에 한 커밋 위로 움직이는 ^(캐럿연산자)
  - 참조 이름에 하나씩 추가할 때마다, 명시한 커밋의 부모를 찾게 됨
  - master^는 "master의 부모"
  - master^^ 는 "master의 부모의 부모"
  - ex) git chekcout master^ // master위에있는 부모를 체크아웃하기
  - HEAD도 적용 가능!!
  - ex
```
git chekcout C3
git checkout HEAD^ // C2가리킴
git checkout HEAD^ // C1가리킴
git checkout HEAD^ // C0가리킴
```
- 방법2) 한번에 여러 커밋 위로 올라가는 ~<num>
  - ex) git checkout HEAD~4 // 네칸 올라감!
```
git checkout C4^ // bugFix의 부모 체크아웃하기 (git checkout bugFix^ 해도 됨)
```


## 2.3 Relative Refs #2 브랜치 강제로 옮기기
- EX) git branch -f master HEAD~3 // (강제로) master 브랜치를 HEAD에서 세번 뒤로 옮기기

- HEAD와 master와 bugFix를 제시되는 골지점으로 옮겨 주십시오.
```
git branch -f master C6 // master가 C6에 감
git branch -f bugFix C0 // bugFix가 C0에 감
git checkout C1 // Head가 C1으로 감
```

## 2.4 Git에서 작업 되돌리기

변경내역을 되돌리는 것도 커밋과 마찬가지로 낮은 수준의 일(개별 파일이나 묶음을 스테이징 하는 것)과 높은 수준의 일(실제 변경이 복구되는 방법)이 있는데, 여기서는 후자에 집중해보자

- Git에서 변경한 내용을 되돌리는 방법
- 1) git reset 쓰기 : git reset은 브랜치로 하여금 예전의 커밋을 가리키도록 이동시키는 방식으로 변경 내용을 되돌립니다. 이런 관점에서 "히스토리를 고쳐쓴다"라고 말할 수 있다. 즉, git reset은 마치 애초에 커밋하지 않은 것처럼 예전 커밋으로 브랜치를 옮기는 것
```
git reset HEAD~1 //한칸위로 돌아감 이러면 로컬 저장소에는 마치 C2커밋이 아예 없었던 것과 마찬가지 상태가 됨
```
- 2) git revert 쓰기
  - 각자의 컴퓨터에서 작업하는 로컬 브랜치의 경우 리셋(reset)을 잘 쓸 수 있지만, "히스토리를 고쳐쓴다"는 점 때문에 다른 사람이 작업하는 리모트 브랜치에는 쓸 수 없다.
  - 변경분을 되돌리고, 이 되돌린 내용을 다른 사람들과 공유하기 위해서는, git revert를 써야한다.
```
git revert HEAD 
// 이상하겠지만, 우리가 되돌리려고한 커밋(C2)의 아래에 새로운 커밋(C2')이 생기게됨. C2라는 새로운 커밋에 변경내용이 기록되는데, 이 변경내역이 정확히 C2 커밋 내용의 반대되는 내용임
// 리버트를 하면 다른 사람들에게도 변경 내역을 push를 보낼 수 있음
```

- Question) Local 브랜치와 pushed 브랜치에 있는 최근 두 번의 커밋을 되돌려 보세요. pushed는 리모트 브랜치이고, local은 로컬 브랜치임을 신경쓰셔서 작업하세요
```
git reset local~1 // local 브랜치니까 reset사용 ㄱㄱㄱ!!!
git checkout pushed // push로 ㄱㄱ
git revert pushed // remote 브랜치는 revert 사용 ㄱㄱㄱ!!! 
```

## 3.1 Git 체리-픽 (Cherry-pick)
현재 위치(HEAD) 아래에 있는 일련의 커밋들에대한 복사본을 만들겠다는 것을 간단히 줄인 말
```
// 형태)
git cherry-pick <Commit1> <Commit2> <...>

// example)
git cherry-pick C2 C4 // -> 해당 위치 아래에다가 C2' C4' 톡톡 떨어트려줌
```

```
git cherry-pick C3 C4 C7 // -> C3' C4' C7' 톡톡톡~~
```

## 3.2 Interactive Rebase Intro
- 원하는 커밋을 모르는 상황에 체리픽 사용 불가능 따라서 다음 방법을 사용함
- 인터렉티브 리베이스 : rebase 명령어를 사용할 때 -i 옵션을 같이 사용하면 됨! 
- 이 옵션을 사용하면 git은 리베이스의 목적지가 되는 곳 아래에 복사될 커밋들을 보여주는 UI를 띄울것임 각 커밋을 구분할 수 있는 각각의 해시들과 메시지도 보여줌
- "실제" git 에서는 UI창을 띄우는것 대신에 vim과 같은 텍스트 편집기에서 파일을 염. 
- 인터렉티브 리베이스 대화창이 열리면, 다음 3가지를 할 수 있음
  - 1) 적용할 커밋들의 순서를 UI를 통해 바꿀수 있다(여기서는 마우스 드래그앤 드롭으로 가능(
  - 2) 원하지 않는 커밋들을 뺄 수 있다. 이것은 pick을 이용해 지정할 수 있다(여기서는 pick토글 버튼을 끄는것으로 가능)
  - 3) 마지막으로, 커밋을 스쿼시(squash)할 수 있다. 불행히도 저희 레벨은 몇개의 논리적 문제들 때문에 지원을 하지 않는다. 요약하자면 커밋을 합칠 수 있다.
  
- Example)
```
git rebase -i HEAD~4 // 창이 뜨고 그것들 선택하면 됨 그러면 그대로 반영해줌
```

- 
```
git rebase -i master~4 --aboveAll
```
