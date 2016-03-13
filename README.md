# FAQ

## Q：如何修改Git过去提交的message？
1. 运行 **git rebase -i ( message hash | HEAD~n)**，选择要修改的message行，将pick改为edit，保存退出；
2. 运行 **git commit -amend -m "new message"**，将新的message提交；
3. 运行 **git rebase --continue**提交修改；
4. 如果已提交到远程仓库，需要运行 **git push <repo> <branch> --force**更新远程git仓库的message。

###References:
https://help.github.com/articles/changing-a-commit-message/
