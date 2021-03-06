# Git Basic 02

> Git Basic 01에서 이어지는 내용이다.



##### 3-5) .gitignore

​		특정 파일 혹은 특정 폴더에 대해 git이 버전 관리를 하지 못하도록 지정하는 것이다.

​		보안을 위해 공유하면 위험한 파일들을 .gitignore에 작성한다.



1. .gitignore 작성 시 주의사항

   - 반드시 이름을 .gitignore로 작성한다.

   - .gitignore 파일은 .git 폴더와 동일한 위치에 생성한다.

   - 제외하고 싶은 파일은 반드시 ``get add`` 전에 .gitignore에 작성한다.

     - 한 번 버전 관리의 대상이 된 파일은 이후에 .gitignore에 작성하더라도 관리의 대상으로 여기기 때문

     

2. .gitignore 쉽게 작성하기

   - 웹사이트

     - [gitignore.io](https://www.toptal.com/developers/gitignore)

       - 웹사이트에서 자신의 운영체제, 개발 툴 등을 검색하면 나오는 문서를 복사하여 붙여넣기하면 된다.

       

   - gitignore 저장소

     - [gitignore](https://github.com/github/gitignore)
       - 링크 접속 후 자신이 사용하는 툴에 맞는 파일을 받는다.



##### 3-6) clone

​		원격 저장소의 커밋 내역을 모두 가져와서 로컬 저장소를 생성하는 명령어

- 다운로드라고 생각하면 편하다
- ``git clone <원격 저장소 주소>``형태로 작성한다.
- ``git clone``을 통해 생성된 로컬 저장소는 ``git init``과 ``gir remote``가 이미 수행되어 있다.



##### 3-7) pull

​		원격 저장소의 변경 사항을 가져와서 로컬 저장소를 업데이트하는 명령어

- 업데이트라고 생각하면 편하다
- ``git pull <저장소 이름> <브랜치 이름>``의 형태로 작성한다.



##### 3-8) Branch

​		git에서 Branch라는 개념은 매우 중요하다.

​		Branch는 나뭇가지처럼 여러 갈래로 작업 공간을 나누어 독립적으로 작업할 수 있도록 도와준다.



- 장점

  1. 브랜치는 독립 공간을 형성하기 때문에 원본을 해치지 않는다.
  2. 하나의 작업은 하나의 브랜치로 나누어 진행되므로 체계적인 개발이 가능하다.

  

- 써야하는 이유

   	1. master 브랜치는 상용을 의미한다. 사용자가 사용하는 부분이라고 생각하면 된다.
   	2. 만약 master 브랜치에서 에러가 생겨 그 에러를 고쳐야 한다면
   	3. 사용자들이 사용하고 있는데 함부로 버전이 되돌리거나 삭제할 수 없다.
   	4. 따라서 브랜치를 통해 별도 작업 공간을 만들고 그 곳에서 작업을 한다.
   	5. 이후에 에러를 해결하면 그 내용을 master 브랜치에 반영한다.
   	6. 이러한 이유로 git에서 브랜치는 강력한 기능 중의 하나라고 할 수 있다.



- git branch

  ​	브랜치 조회, 생성, 삭제 등 브랜치와 관련된 git 명령어

  

  - 명령어

    - ``git branch``: 브랜치 목록 확인
    - ``git branch <브랜치 이름>``: 새로운 브런치 생성
    - ``git branch -d <브랜치 이름>``: 특정 브랜치 삭제

    

- git switch

  현재 브랜치에서 다른 브랜치로 HEAD를 이동시키는 명령어.
  
  HEAD란 현재 브랜치를 가리키는 포인터를 의미.
  
  
  
  - 명령어
    - ``git switch <다른 브랜치 이름>``: 다른 브랜치로 이동
    
    
  
- Branch Merge

  각 브랜치에서 작업이 끝났을 때 그 작업 내용을 합치기 위해 사용

  

  - git merge

    분기된 브랜치들을 하나로 합치는 명령어

    ``git merge <합칠 브랜치 이름>``의 형태로 사용

    merge하기 전에 일단 다른 브랜치를 합치려고 하는, 즉 메인 브랜치로 switch해야한다.

    

  - merge의 세 종류

    1. Fast-Forward

       - 브랜치를 병합할 때 마치 빨리감기처럼 브랜치가 가리키는 커밋을 앞으로 이동시키는 것

       

    2. Auto-Merging

       - 브랜치를 병합할 때 각 브랜치의 커밋 두개와 공통 조상 하나를 사용하여 병합하는 것
       - 두 브랜치에서 *다른 파일* 혹은 *같은 파일의 다른 부분*을 수정했을 때 가능하다.

       

    3. Conflict

       - 병합하는 두 브랜치에서 *같은 파일의 같은 부분*을 수정한 경우, git이 어느 브랜치의 내용으로 작성해야 하는지 판단하지 못해서 발생하는 충돌 현상
       - 결국 사용자가 직접 내용을 선택해서 충돌을 해결해야함.

---

### 4. git workflow(원격 저장소 소유권이 없는 경우, Fork & Pull model)

---

##### 4-1) 개념

- 오픈 소스 프로젝트와 같이, 자신의 소유가 아닌 원격 저장소인 경우 사용
- 원본 원격 저장소를 그대로 내 원격 저장소에 복제(=fork)
- 기능 완성 후 복제한 내 원격 저장소에 push
- 이후 Pull Request를 통해 원본 원격 저장소에 반영될 수 있도록 요청



##### 4-2) 작업 흐름

1. 소유권이 없는 원격 저장소를 fork를 통해 내 원격 저장소로 복제
2. fork 후, 복제된 내 원격 저장소를 로컬 저장소에 clone.
3. 이후 로컬 저장소와 원본 원격 저장소를 동기화하기 위해 연결.
 - ``git remote add upstream <원본 원격 저장소 주소>``
4. 사용자는 자신이 작업할 기능에 대한 브랜치를 생성하고, 그 안에서 기능 구현
5. 기능 구현이 완료되면, 복제 원격 저장소에 해당 브랜치를 push
6. Pull Request를 통해 복제 원격 저장소의 브랜치를 원본 원격 저장소의 master에 반영해달라는 요청을 보냄
7. 원본 원격 저장소의 master에 브랜치가 병합되면 복제 원격 저장소의 브랜치는 삭제. 그리고 사용자는 로컬에서 master 브랜치로 이동
8. 병합으로 인해 변경된 원본 원격 저장소의 master 내용을 로컬에서 받아옴. 그리고 기존 로컬 브랜치는 삭제.

