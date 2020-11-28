# ⚛️ 깃의 중요한 컨셉 이해하기
- 깃은 파일!
- working directory : 작업중 [untracked, tracked]
- staging area : 커밋준비 완료상태
- .git directory : 커밋후 히스토리에 저장 -> checkout -> 돌아가기 가능!
- remote : gitgub(pull, push), bitbucket....
- commit : hashcode [message, autore, data/time]

## 1. 🌟 깃 초기화 하고 삭제 하기
1. git init
2. 폴더에서 .git 파일 삭제!
```
//초기화 하기
git init
ls -al// .git 파일을 확인할 수 있다.
//삭제 하기
rm -rf .git
```
> alias 설정하기
> git config --global alias.st status // git st로 -> git status 사용할 수 있어요

## 2. 깃 로컬 파일들 추가하기 add
1. git add [., *, *.txt]
```
echo hello world1 > a.txt
echo hello world2 > b.txt
echo hello world3 > c.txt

git add *.txt // -> staging area => ready to commit
echo welcome >> a.txt //update => modified
git add a.txt

git rm --cached // from stage to untracked

```
> .gitignore 만들어서 tracking안되게 만들기

## 3. 깃 현재상태 확인하기 status
1. git status // --long 디폴트
```
git status -s // --short 
```
## 4. 📏 깃 파일 비교하기 diff
- 어떤 파일이 수정되었나?? 
1. git diff // working directory에 있는 상태를 확인 할 수 있어요 이전 커밋 or staging area와 비교
```
git diff
이전   지금
@@ -1 +1,2@@

git diff --stage // staging area와 이전 커밋과의 비교
```
> git config --global -e

## 5. 🗄 깃 버전 등록하기 commit
1. git commit
	- title & message
```
git add .
git commit -m "message1"

git commit -am "message all" // working directory 모두! 커밋하자
```
> 어떤 규모의 커밋? 깃은 히스토리의 창고!! 
어플리케이션 기능별 작업별로 관리하는 것이 중요!
💥 현재형 동사로 만든다.
1. init 
2. Add login
3. Add user
4. Add welcome 

> 바로 staging area로 가자!! 🛒
git rm 
git mv 

## 6. 🗂 버전들 목록 보기 log
1. git log
```
git log --p //수정 내용확인 가능
git log --oneline //간단한 커밋 메세지 
git log --oneline --reverse //거꾸로 보기
git log --pretty=oneline
git log --pretty=format:"%h %an"
[깃 로그 설정](https://git-scm.com/docs/git-log)

git log --oneline --graph --all

git log --oneline -3 // 최근 커밋 3개 까지만 
git log --author="gil"
git log --before="2020-12-25"
git log --grep="project" //title에서 있다면
git log -S "문자열" //커밋 내용안에 문자열이 있는 커밋 
git log -S "문자열" -p // 코드 내용을 볼 수 있다.

```
> HEAD는 내가 지금 보고 있는 위치!!
head~1 // 이전 
head~2 // 이전 이전

>git config --global alias.ll "log --graph --all --pretty=format:'%C(yellow)[%ad]%C(reset) %C(green)[%h]%C(reset) | %C(white)%s %C(bold red){{%an}}%C(reset) %C(blue)%d%C(reset)' --date=short"

## 6. 🔖 깃 태그 tag
-  특정 커밋을 북마크해서 빠르게! 릴리즈 버전v(major.minor.fix)을 태그별로 관리하자
1. git tag "태그 이름" hashcode
2. git tag "v2.1.1" hashcode -a[anotate] -m "message"
	- git tag "v2.*"
    - git tag -d 태그이름
3. git show 태그이름 
4. git checkout 태그이름


# Branch 💥 브랜치 💥 
- 협업의 필수! link - point
	- master nono🔴
    - feature yes🟢
- merge only commit 🎇nice
- delete branch

## ▶️ 브랜치 기본사용
### 1. git branch
1. local의 branch 목록확인 가능
2. git branch --all 서버의 모든 브랜치 확인 가능
3. git checkout new-branch //동일한 커밋을 가르키는 새로운 브랜치 생성
4. git switch new-branch vs git checkout new-branch //새로운 브랜치로 이동
	- git switch -C new-branch 
    - git checkout -b new-branch // 브랜치 만들면서 바로 이동
5. git branch -d new-branch //삭제하기
	- git push origin --delete new-branch
 	- git push origin :new-branch
6. git branch --move fix fix-welcome
	- git push --set-upstream origin --fix-welcome
 
### 2. git merge
- fast-forward merges
	- 마스터 브랜치에서 새로운 브랜치가 생긴 이후에 변화가 없다면 가능!
    - 단순 포인터를 변경! ✨ 짜란 ✨

1. git merge branch-name
```
git merge branch-name
git merge --no-ff branch-name //fast-forward 하고싶지 않으면! 머지 커밋 생김

```

- three-way mergets
	- fast-forward불가능한 상태에서 이루어 지는 커밋
    - 새로운 머지 커밋이 생긴다.

> 충돌(conflict)
동일한 파일, 동일한 부분을 수정하게 되었다면 발생.

- 충돌 conflict
```
git merge branch //충돌 발생.
git mergetool //vscode로 셋팅후 [git config --global -e]
git add . //수정 완료후
git merget --continue //머지 계속진행

```
![git config --global -e](https://images.velog.io/images/jgi0105/post/24a1459e-0712-4807-88f9-380109117b31/image.png)
> TIP: 오리지널 파일 nono
git config --global mergetool.keepBackup false

### 2. 🐝 git rebase
- 현 브랜치의 베이스를 변경하게 된다면? fast-forward를 할 수 있다.
- 🔴 주의!! 리베이스의 경우에는 새로운 커밋을 만들어서 베이스를 변경하는 것으로! 나 혼자만 작업하는 브랜치만!!!!!!

1. git rebase
2. git rebase --onto master develop feature
	- develop에서 파생된 feature브랜치를 master로 리베이스 하겠다!
```
//master new-branch
git rebase master // new-branch가 master의 최신의 커밋으로 변경
```
### 2. 🍒 git cherry-pick
- 특정 커밋만 딱! 가져와
- 작업기간이 오래 걸리는 작업에서 특정 기능(커밋)을 빼올때 유용!
```
//master branch
git cherry-pick hash
```

# Stash 💥 스태시 stack 💥
- working directory에 있는 나의 작업들!!.. 앗 아직 커밋 단계는아니야❗️❓
- 잠시 저장!! 얍⚠️

1. git stash
2. git stash list //stash stack 목록 보기
3. git stash apply(그냥 적용) id or pop(빼면서 적용)
4. git stash drop [id]//그냥 제거
5. git stash clear //전체 삭제

```
git stash [push] -m "message"
git stash list

//지금 스테이징에 올라가 있는 상태를 유지하고 stash stack에 push as well
git stash push -m "message" --keep-index 

//If you want to push untracking code into stash stack as well, Use -u option.
**텍스트**
git stash -u
```

# Reset 🛁 헛... fix 하자 

## 1. git restore >2.23.0
- 변경사항을 제거하기
1. git restore .
	- 전체적인 내용 초기화
- staging area에 있는 내용을 working directory 가져오기
2. git resotre --staged .
```
// 해당 내용까지 삭제!
git restore --source=hash코드
or
git restore --source=HEAD~1
```

## 2. 커밋 수정하기 amend
1. git commit --amend // 잘 못된 커밋 수정하기.
	- ‼️서버에 업로드 하지 않았을때 사용하기!!!
```
//message
git commit --amend -m "new message"

//내용 추가하기
//방금 반든 커밋에서 다시 수정하기...
~~~~
git add .
git commit --amend
```

## 2. 커밋을 초기화 시키자 reset
1. git reset 원하는 커밋 위치!
	- git reset HEAD~2
    - git reset hash
```
git reset // --mixed  (기본)커밋은 삭제 하지만 내용은 working directory 옮긴다.
git reset --soft HEAD~1 // --soft : staging area로 가져온다.
git reset --hard HEAD// 그냥 커밋 + 내작업 내용 다!!! 삭제해줘 🗑‼️
```

2. 💝 git reflog => reset 되살리기!
	- 지금까지 실행된 내용을 모두 담고 있어요!
    - git reset --hard hash 🤩 다시 부활!
    
> 🔱 vscode local hisotry!! extension good!!
⚜️ webstorm은 기본으로 있어요!.....

> git clean -fd
트래킹 되지 않는 새로운 파일은 restore되지 않기 때문에 clean을 사용.



## 3. 커밋을 초기화 시키자 reset
1. git reset 원하는 커밋 위치!
	- git reset HEAD~2
    - git reset hash
```
git reset // --mixed  (기본)커밋은 삭제 하지만 내용은 working directory 옮긴다.
git reset --soft HEAD~1 // --soft : staging area로 가져온다.
git reset --hard HEAD// 그냥 커밋 + 내작업 내용 다!!! 삭제해줘 🗑‼️
```

> git clean -fd
트래킹 되지 않는 새로운 파일은 restore되지 않기 때문에 clean을 사용.

## 4. 커밋을 초기화 revert
- 변경사항을 다시 없애 주는, 삭제해주는, 쥐소해주는 새로운 커밋을 만든다.
	- 딱! 그 커밋에서 수정한 내용을 제거한 다음 새로운 커밋을 만들어 준다.
1. git revert hash
2. git revert --no-commit hash
	- 바로 커밋을 하지 않고 취소 되는 변경사항을 staging area에 올려준다.

> [[revert]]는 이미 마스터 브랜치에 커밋된 아이들이라면, reset또는 rebase보다 revert를 하는것이 맞습니다. [[revert]]는 새로운 커밋을 만들어서 이미 추가된 내용을 변경하는 것이므로 
즉❗️ 히스토리를 수정 하지 않기 때문에 언제든지 자유롭게 이용할 수 있다.


## 5. 이전 커밋 수정하기 interactive rebasing
- 🔴 rebasing을 하는 순간 현재 rebasing이후의 커밋은 새로운 히스토리가 된다!!
	- 전부 새로운 아이템이 된다!!!!
	- 내가 가고 싶은 이전보다 하나더 ◀️ 앞!!! 으로 rebasing해야 한다.
1. git rebase -i hash
```
//hash값 이후나온 커밋들을 함께 rebase할 거다!
git rebase -i hash

pick: use commit
reword: use commit, but edit the commit message
edit: use commit, but stop for ameding // 컷밋을 쓰지만 안에 변경 사항을 바꾸겠다.
squash: use commit, but meld into previous commit //커밋을 하나로 묶는다.
fixup: squash랑 같지만 메세지 안남김.

💥drop을 해서 충돌이나면
git add .
git rebase --continue를 해줘용

```
