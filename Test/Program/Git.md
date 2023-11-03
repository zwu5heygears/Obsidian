配置
```bash
git config --list
git config --global user.name "[name]"
git config --global user.email "[email]"
git config --global color.ui auto
```
初始化
```bash
git init
git clone [url]
```
状态查询和提交
```bash
git status
git add [file]
git reset [file]
git reset --hard [commit]
git diff  # 工作区和暂存区
git diff --staged  # 暂存区和版本库
git diff branchB..branchA
git show [SHA] # 节点具体修改信息
git commit -m "[info]"
git rm [file] / rm --f [file] # 不追踪且删除文件
git rm --cached [file] # 不追踪文件not add 
```
分支合并
```bash
git branch
git branch [name]
git branch -d [name]
git checkout [branchname]
git merge [branch]
git rebase [barnch]  # 变基
```
日志
```bash
git log
git log branchA..branchB
git log --follow [file]
git log --stat -M
```
暂存
```bash
git stash # 暂存修改到工作区，而不进行add commit
git stash list
git stash pop
git stash drop
```
文件管理
```bash
git rm [file]
git mv [old-file] [new-file] # 修改文件名加移动文件
```
远程仓库
```bash
git remote add [alias] [url]
git fetch [alias]
git merge [alias]/[branch]
git push [alias][branch]  # 一般只有一个alias 直接branch
git pull
```
子仓库创建
```bash
git submodule add [submodule address]
git commit -m "add submodule"
```
子仓库拉取
```bash
git submodule init
git submodule update
# 整合
git submodule update --init --recursive
```
子仓库更新
```bash
cd submodule_folder
git pull origin master
// 更新全部子仓库
git submodule foreach 'git pull origin master'
// 全部子仓库指定切换分支
git submodule foreach 'git checkout xxx'
```
删除子仓库
```bash
# 卸载子仓库
git submodule deinit [submodule_folder]
# 移除子库的本地修改
git submodule deinit [submodule_folder] --force
# 删除文件夹
git rm [submodule_folder]
# 提交
git commit -m "delete submodule xxx"
```
Tag
```bash
git tag
git tag v1.0
git tag -a v1.2 -m "info"
git push origin v1.0
```


```bash
git config --list 
git config --global user.name ""
git config --global user.email ""

git init
git clone url
git clone -b [branch] [url]

 
git add .
git add img/*.cpp
git checkout file
git rm --cached file

git reset HEAD
git reset HEAD file
git reset --hard file
git reset --soft file 
git reset --mixed file

git checkout master
git checkout hashid
git checkout -


git status
git status -s
git status -b dev
git status --show-stash
git status --long
git status -v -v
git status -uno
git status -unormal
git status -uall
git status --ignore-submodules
git status --renames
git status --no-renames

git log
git log -p
git log --oneline
git log --graph
git log --decorate
git log --reverse
git log --author=zwu5
git log --since={1.hours.ago} --after={2023-09-17}
git log --grep=update
git log --no-merges
git log --stat
git log --abbrev-commit

git diff
git diff file
git diff --stat
git diff --cached 
git diff --cached file
git diff --staged 
git diff --staged file
git diff HEAD^ HEAD
git diff master dev


git commit -a -m ""
git commit -m ""
git commit file1 file2 -m ""
git checkout HEAD file

git reset hashid/HEAD^
git reset --soft HEAD^
git reset --mixed HEAD^
git reset --hard HEAD^

git rm file 
git rm --cached file

git mv file newfile
git mv -f file newfile

git checkout dev
git checkout -b dev3
git checkout -
git checkout file = git restore file
git checkout -- file
git checkout hashid
git checkout master


git tag
git tag v1.0
git tag -a v2.0 -m ""
git show tag v2.0
git tag -f v1.0 hashid
git ls-remote --tags origin
git tags -l v1.*
git checkout v1.0
git push origin v1.0
git push --tags
git tag -d v2.0
git push --delete origin v2.0

git blame file
git blame -L 2,10 file

git switch dev
git switch -c dev3
git switch -

git restore file
git restore .
git restore --staged file
git restore --source=hasid file


git remote 
git remote -v
git remote add branch url
git remote rename origin main
git remote remove origin
git remote set-url origin url
git remote show origin

git stash
git stash list
git stash apply 0
git stash pop 0
git stash drop 0
git stash show
git stash show -p
git stash branch rr


git fetch
git merge origin/master
git pull
git pull origin master
git pull --rebase ()

git push
git push origin master
git push --force origin master
git push --set-upstream origin temp2
git push origin --delete temp2


git rebase master
git rebase --abort
git rebase --continue

git merge (--ff)
git merge --no-ff
git merge --squash  ---> git commit 
git merge --abort
git merge --continue

git submodule add url path  ---> git commit // 直接添加子模块
git clone url --recursive
git submodule init
git submodule update
git submodule status

// 主模块
git submodule update
// 子模块
git pull


rm -rf submodulepath
vim .gitmodule
vim .git/config
rm .git/module/*
git commit -a -m ""

git diff > patchfile
git format-patch -1 hasiID
git apply --stat patchfile
git apply --check patchfile
git apply patchfile1 patchfile2  ---> add commit
git am patchfile1 patchfile2


git cherry-pick hashid1 hashid2
git cherry-pick --contiue
git cherry-pick --abort

git revert hasid

git bisect start HEAD hashid
git bisect good/bad
git bisect reset

git grep code
git grep -e regex


git merge-base dev dev2

git push
git push -f

git commit --amend
```