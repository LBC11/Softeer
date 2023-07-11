### git의 구조
- 각 commit(자식)은 이전 commit(부모)을 가리키는 link를 통해 연결되는 LinkedList의 형태를 가지고 있다.
- 자식이 부모를 가리키는 이유는 git에서 객체는 불변이기 때문에 먼저 만들어진 부모 commit의 상태를 변경할 수 없기 떄문이다.
- 각 commmit은 tree형태의 객체를 가지고 있는데, 이는 stage에서 commit을 할 때 저장된 작업물을 가지고 만들어진다.
- tree의 각 node는 Blob(Binary large object)를 가지고 있는데, 이는 git에서 작업물을 저장한 객체이다.

### git의 명령어
- working directory, stage(임시 논리적 저장소), local 저장소(.git folder) 이 3개를 중심으로 각 명령어가 어떠한 영향을 주는지 이해하는 것이 중요하다.
- init: 현재의 directory를 working directory로 만들어주어 앞으로 git 명령어를 사용할 수 있게 해준다.
- add: working directory의 작업물을 Blob으로 만들어 stage에 저장한다.
- commit
  + 특정 directory안의 stage에 저장되어 있는 Blob으로 tree를 만들고 commit에 붙여서 local 저장소에 저장한다.
  + 새롭게 선언된 commit을 HEAD branch가 참조하게 한다.
  + 자식 commit이 이전에 HEAD가 참조하던 부모 commit에 link를 연결하게 한다.
- switch: 특정 commit의 tree를 불러오는 행위
- status
  + untracked: working directory에만 작업물이 있는 상태
  + modified: working directory, stage에 같은 작업물이 존재하고 local 저장소에는 없는 상태
  + nothing: working directory, stage, local 저장소 모두 같은 작업물을 가지고 있는 상태
- push
  + 특정 commit을 원격 저장소에 저장한다.
  + 현재 branch에 속한 commit만 저장된다.
- fetch: 원격 저장소에 있는 모든 commit을 local 저장소에 가져오는 명령어
- merge: 현재 branch의 마지막 commit과 대상 branch의 마지막 commit을 합쳐서 새로운 commit을 만든다.
- pull: fetch + merge
- rebase
  + 현재 branch의 commit(공통되는 commit을 제외한)들의 내용을 그대로 복사하여 새로운 commit들을 만들고 그 commite들을 대상 branch의 마지막 commit뒤에 옮겨서 붙이는 명령어
  + git history를 깔끔하게 정리하는 효과가 있다.
  + merge와 같이 쓰면 git history가 꼬일 수 있으니 조심하자!!!

### git의 객체
- commit, Blob, tree
- 각 객체의 hash값은 file의 내용을 기반으로 한다.
- 파일 이름이 다르고 생성 시점이 달라고 내용이 같다면 hash값이 같다.

### git branch
- branch는 참조일 뿐 객체가 아니다.
- HEAD branch는 현재 위치의 branch를 가리키는 또 다른 참조일 뿐이다.
- fast forward merge: 새로운 commmit이 생성되지 않고 HEAD branch가 가리키는 것만 갱신되는 merge
- ref log를 통해 HEAD branch가 참조했던 commit들의 hash값을 확인할 수 있다.
- 위를 이용해 git history가 꼬이면서 현재 보이지 않는 commit들을 찾아서 꼬인 것을 해결할 수 있다.

### git에 대한 오해
- commit을 한다고 해서 stage에 저장되어 있던 작업물이 삭제되는 것이 아니다. 그대로 남아있고 이를 통해 status가 결정된다.
- git에서 저장할 때 변경사항만 저장하는 것이 아니다. 변경사항만 저장한다면 특정 시점으로 돌아갈 때 그 중간의 모든 commit을 일일이 방문하여 변경사항들을 불러와야 하기에 시간이 오래 걸린다.
