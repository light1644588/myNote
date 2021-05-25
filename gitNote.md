### Git

  - 版本回退 
    - git reset --hard HEAD^ 回退上一个版本
    - git reset --hard HEAD^^ 回退上上个版本
    - git reset --hard HEAD~n 回退上n个版本
    - git reset --hard uuid 版本到 第uuid个版本

  - 版本删除
    - git rm [<文件名>]

  - 添加远程仓库 并添加别名
    - git remote add origin https://github.com/xxx/xxx/.../xxx.git

  - 将本地仓库推送到远程仓库
    - git push -u origin master

  - 使用本地git客户端生成ssh公钥和私钥 
    ```git
      ssh-keygen -t rsa -C 'githubEmail'
    ```

  - 检查测试链接
    - ssh -T git@github.com


  - 分支操作
    | 命令  | 描述  |
    | :---- | :----  |
    | git checkout branch | 切换指定分支 |
    | git checkout -b new_branch | 新建分支并切换到新建分支 |
    | git branch -d branch | 删除指定分支 |
    | git branch | 查看所有分支，并且*标记当前所在分支 |
    | git merge branch | 合并分支 |
    | git branch -m | -M old branch new branch | 重命名分支，若分支已经存在，则使用-M强制重命名，否则，使用-m重命名 |

  测试分支操作
  ##### 重点
  - 主干上合并分支

  - 分支push和pull操作
    | 命令  | 描述  |
    | :---- | :---- |
    | git branch -a | 查看本地与远程分支 |
    | git push origin brach_name | 推送本地分支到远程 |
    | git push origin :remote_branch | 删除远程分支（本地分支还在保留） |
    | git checkout -b local_branch origin/remote_branch | 拉取远程指定分支并在本地创建分支 |

  - 分支操作冲突出现于解决
    开发中不同分支下同一文件修改后执行就会出现问剑修改冲突情况，这里说明一种比较常见的冲突问题，以master和leaf01两个分支进行演示说明。

    避免冲突解决：先拉去再推送

  - 本地分支操作冲突
    修改完本地分支后即可提交，执行命令查看分支合并图 git log --graph --pretty=oneline


  - 标签管理
    | 命令  | 描述  |
    | :---- | :---- |
    | git tag tag_name | 新建标签默认未HEAD |
    | git tag tag_name -m 'xxx' | 添加标签并指定标签描述信息 |
    | git tag | 查看所有标签 |
    | git tag -d tag_name | 删除一个本地标签 |
    | git push origin tag_name | 推送本地标签到远程 |
    | git push origin --tags | 推送全部未被推送的标签到远程 |
    | git push origin :refs/tags/tag_name | 删除一个远程分支 |