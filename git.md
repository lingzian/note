`说说对git pull 和 git fetch 的理解？有什么区别？`
git fetch是将远程主机的最新内容拉到本地，用户在检查了以后决定是否合并到工作本机分支中
git pull 则是将远程主机的最新内容拉下来后直接合并 git pull = git fetch + git merge



`说说你对git rebase 和 git merge的理解？区别？`
merge
通过merge合并分支会新增一个merge commit，然后将两个分支的历史联系起来 其实是一种非破坏性的操作，对现有分支不会以任何方式被更改，但是会导致历史记录相对复杂

rebase 会将整个分支移动到另一个分支上，有效地整合了所有分支上的提交 主要的好处是历史记录更加清晰，是在原有提交的基础上将差异内容反映进去，消除了 git merge所需的不必要的合并提交



`说说你对git reset 和 git revert 的理解？区别？`
git revert是用一次新的commit来回滚之前的commit，
git reset 用于回退版本，可以遗弃不再使用的提交 是直接删除指定的commit