# Instruction
```
>>github로 파일 올리기.

git add 파일이름

git status

git commit -m 원하는 커밋이름

git push
```
```
>>github에서 수정된 파일 내려받기.

git pull

```
```
>>github에 올리고 싶은 프로젝트 폴더에서 올리는 방법


git remote add origin git@github.com:<아이디>/git-practice.git


git push --set-upstream origin main

```
```
>>git에서 주소를 받아 새로운 파일을 만들때

gitdptj 주소를 받는다.

저장하기 원하는 디렉터리에서 git bash 터미널 선택

git clone 주소
```

```
컴퓨터에서 git commit 하지 않은 작업이력은  
git restore 파일명 으로 최신 저장된 상태로 돌아갈 수 있다.

git reset 커밋주소  
:git log로 각 커밋의 주소를 알고, 그 상태로 돌아가고 싶은 것을 선택 해햐 한다. 삭제할 커밋을 선택하는 것이 아니라 돌아가고 싶은 커밋의 주소를 선택해야 한다.

```
```
>>주의사항
서버에 올리기 전에 git pull을 먼저 실행해주는게 좋다.
왜냐하면 서버에서(github) 수정을 하고 있는 상황일 수 있고 수정한 것을 pull하지 않은 상황일 수 있다.
그러면 로컬에서의 상황과 서버에서의 상황이 달라 push 할 수 없다.
그래서 먼저 git pull 해주면 수정한 것을 pull하지 않은 상황을 대비할 수 있게된다.
git config pull.rebase false //오류메시지

```
