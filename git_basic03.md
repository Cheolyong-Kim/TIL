# Git Basic 03

> Git Basic 02에서 이어지는 내용이다.



### 5. Undoing

---

##### 5-1) 파일 내용을 수정 전으로 되돌리기

> *Working Directory*에서 파일을 수정했다고 가정. 
>
> 이 파일의 수정 사항을 취소하고, 원래 모습으로 돌리려고 할 때 사용



1. git restore
   - ``git restore <파일 이름>``의 형식으로 사용.
   - 단, 버전 관리가 되고 있는 파일만 되돌리기가 가능.
   - 한 번 git restore를 통해 수정을 취소하면, 해당 내용을 복구할 수 없음.



##### 5-2) 파일 상태를 Unstage로 되돌리기

> *Staging Area*와 *Working Directory* 사이를 옮겨다닐 수 있는 방법.
>
> ``git add``를 통해 파일을 *Staging Area*에 올렸다고 가정.
>
> 이 파일을 다시 Unstage 상태 즉, *Working Directory*로 돌아가려할 때 사용.



1. git rm --cached

   - ``git rm --cached <파일 이름>``의 형식으로 사용.
   - *커밋이 하나도 없는 상태*에서 사용하는 명령어 

   

2. git restore --staged 

   - ``git restore --staged <파일 이름>``의 형식으로 사용.
   - *커밋이 존재하는 경우*에 사용하는 명령어



##### 5-3) 바로 직전 커밋 수정하기

> A라는 기능을 완성하고 "A기능 완성''이라는 커밋을 남겼다고 가정.
>
> 이 때 A기능에 필요한 파일 중 1개를 빼놓고 커밋했을 때 직전 커밋을 취소하고, 
>
> 다시 커밋할 때 사용하는 명령어



1. git commit --amend

   - *Staging Area*에 새로 올라온 내용이 없다면, 단순히 직전 커밋의 메시지만 수정한다.

   - *Staging Area*에 새로 올라온 내용이 있다면, 직전 커밋 내역에 같이 묶어서 재커밋된다.

     

     i. 커밋 메시지만 수정하는 경우

     		- ``git commit --amend``를 입력하면 Vim편집기가 열림
     		- Vim편집기 내부에서 커밋 메시지 수정
     		- 커밋 메시지를 수정하고 저장하면 새로운 메시지로 변경되며 커밋 해시 값 또한 변경됨.

     

     ii. 커밋 재작성

     - 누락된 파일을 Staging Area로 이동시킴
     - ``git commit --amend``를 입력하면 Vim편집기가 열림
     - Vim편집기를 저장 후 종료하면 직전 커밋이 덮어 씌워짐. 마찬가지로 커밋 해시 값 또한 변경됨.

     

     iii. Vim 간단 사용법

     - 입력 모드 ``i``
       - 문서 편집을 가능하게 해줌
     - 명령 모드 ``esc``
       - ``:wq``: 저장 및 종료 (w= 저장, q=종료)
       - ``:q!``: 강제 종료 (q=종료, !=강제)

---



### 6. reset, revert

> 이미 업데이트를 했는데, 예전 버전으로 돌아가고 싶을 때 사용하는 명령어



##### 6-1) git reset

- ``git reset [옵션] <커밋 ID>``의 형태로 사용
- 과거로 돌리는 듯한 행위로, 특정 커밋 상태로 되돌아감.
- 특정 커밋으로 되돌아갔을 때 해당 커밋 이후로 쌓아 놨던 커밋들은 전부 사라짐.

- 옵션
  - ``--soft``
    - 돌아가려는 커밋으로 되돌아가고
    - 이후의 커밋된 파일들을 *Staging area*로 돌려놓음
    - 즉, 다시 커밋할 수 있는 상태가 됨
  - ``--mixed``
    - 돌아가려는 커밋으로 되돌아가고
    - 이후의 커밋된 파일들을 *Working directory*로 돌려놓음
    - 즉, unstage된 상태로 남아잇음
    - 기본 값
  - ``--hard``
    - 돌아가려는 커밋으로 되돌아가고
    - 이후의 커밋된 파일들은 모두 *Working directory*에서 삭제
    - 이미 삭제한 커밋으로 다시 돌아가고 싶다면 ``git reflog``를 사용



##### 6-2) git revert

> git reset은 쉽게 과거로 돌아갈 수 있다는 장점이 있지만, 커밋 내역이 사라진다는 단점이 있음.
>
> 따라서 다른 사람과 협업할 때 커밋 내역의 차이로 인해 충돌이 발생할 수 있음.



- ``git revert <커밋 ID>``의 형태로 사용
- 특정 사건을 없었던 일로 만드는 행위로써, *이전 커밋을 취소한다는 새로운 커밋*을 만듬
- git reset은 커밋 내역을 삭제하는 반면, git revert는 새로 커미을 쌓는다는 차이가 있음

---



### 7. ETC

> 나머지 도움이 될만한 자료들



[좋은 커밋 메시지 작성을 위한 규칙](https://beomseok95.tistory.com/328)

[커밋 메시지 작성 규칙](https://gist.github.com/stephenparish/9941e89d80e2bc58a153)