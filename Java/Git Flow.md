tags: #git

---

$ git clone -b `branchname` `remote-repo-url`
$ git flow init  → 쭉 엔터

작업 시작
**_$ git flow feature startISSUE-번호_**

작업 후 
**$ git add .**
**$ git commit -m ‘이슈번호 blabla..’**
**_$ git checkout develop_**
**_$ git pull origin develop_**

→ Already up to date : 내 feature가 최신일 경우 
***$ git checkout feature/***ISSUE-번호
***$ git flow feature finish*** ISSUE-번호 
***$ git push origin develop***

→ pull 왕창 받음!
**_$ git checkout feature/ISSUE-번호_**
**_$ git rebase develop_**
 
rebase 후 conflict 발생 시 conflict 수정 
$ git add .
$ git commit -m ‘blabla…’
***$ git rebase --continue***

rebase OR conflict 수정 후
***$ git flow feature finish ISSUE번호***
***$ git push origin develop***


---

작업 브랜치 생성
git branch ISSUE-xx

작업 브랜치 이동
git checkout ISSUE-xx

작업 후
git add .
git commit -m ‘개발 내용’

메인 브랜치 이동
git checkout main
git pull
git merge ISSUE-xx

conflict 발생 시 수정 후 
git add .
git commit -m ‘resolved conflict’
git push

(작업 추가 필요하면 브랜치 삭제 전 작업 브랜치 이동
git checkout ISSUE-xx
git merge main
git push origin ISSUE-xx)

서버에서 브랜치 삭제 (push안했으면 안해도돼)
git push origin —delete ISSUE-xx

로컬에 브랜치 삭제
git branch -d ISSUE-xx

→ 브랜치 삭제 참고하기
https://www.freecodecamp.org/korean/news/how-to-delete-a-git-branch-both-locally-and-remotely/

