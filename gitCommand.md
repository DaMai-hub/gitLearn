# gitNote

## (1)<font color=#FFE4B5>git_init</font>

  * 初始化流程

    ```
    git init <project-name>
    创建一个新的本地仓库(repository)[省略project-name则在当前目录创建]
    git remote add origin <url>
    本地项目就和远程仓库建立了连接，git fetch, git pull origin master拉取库上文件

    git config --global --list
    查看当前git文件的全局配置

    git config --global core.autocrlf true/input/false
    配置git pull/push 的换行符风格
    * true : 在检出代码时自动将行尾转换为CRLF(windows换行符)，在提交代码时自动将行尾转换为LF(linux/mac/unix换行符)
    * input : 在检出代码时不自动转换行尾，在提交代码时自动将行尾转换为LF
    * false : 在检出和提交时都不自动转换行尾

    git config --global user.name "your name"
    配置用户名

    git config --global user.email "mail@example.com"
    配置邮箱

    git clone <url>
    克隆一个远程仓库，无需git init，在当前目录会自动新建一个库名字文件夹，里面会自动git init
    git clone <url> .
    克隆一个远程仓库，放置当前目录，不会新建库名字文件夹
    git clone <url> fileName
    克隆一个远程仓库，放置在当前目录下，名字是"fileName"的文件夹中

    详细流程参考"Git配置以及从GitHub上克隆项目.html"

    cat ~/.ssh/id_rsa.pub
    显示ssh key

    指令创建SSH Key
    step-1: ssh-keygen -t rsa -C <mail@example.com>
    直接输入邮箱，不要带双引号之类的

    显示创建成功:
    Generating public/private rsa key pair
    Enter file in which to save the key
    Create directory

    step-2: Enter passphrase(empty for no passphrase)
    让你输入密码，如果你设置了密码
    在使用ssh传输文件的时候，立即要输入这个密码
    为了避免麻烦，建议不用设置，直接回车

    step-3: Enter same passphrase again
    让你再次输入密码
    如果上一步没有设置密码，直接回车就可以了

    github添加SSH时，提供SSH with authentication key认证方式
    此方式就是step2中使用key之前需要先输入passphrase
    如果没有passphrase，这是完毕后用户的任何git操作都不需要输入passphrase

    git bach配置
    鼠标选择+右键直接复制

    推荐vscode插件
    "GitLens-Git supercharged" 可视化git流程[收费]
    替代产品: "VSCode替代GitLens.html"
    "git Blame"/ "git graph"/ "git history"

    vscode 通过ssh连接虚拟机
    参考"vscode连接虚拟机服务器ssh.html"
    ubantu统一时间
    参考"虚拟机与宿主机的时间.html"
    vscode通过ssh连接到虚拟机无法进行命令操作
    参考"VScode远程连接虚拟机终端install错误，无法command.html"

    vscode terminal如何能在输出信息的同时把信息记录到文件中
    "对应指令(e.g ls/python...) | tee xx.txt"
    ```

    ![Alt text](image/git_24.png)
    ![Alt text](image/git_25.png)
    ![Alt text](image/git_26.png)
    ![Alt text](image/git_27.png)

  * 目录区域划分

    ```
    工作区(working directory): 在电脑中能实际看到的目录

    暂存区(stage/index): 暂存区也叫索引，用于临时存放未提交的内容，一般在.git目录下的index中

    本地仓库(repository): git在本地的版本库，仓库信息存储在.git隐藏目录中

    远程仓库(remote repository): 托管在远程服务器上的仓库，常有github, gitlab, gitee
    ```

    ![Alt text](image/git_01.jpg)


  * git 3种状态/基本概念

    ```
    已经修改(modified): 修改了文件，但没保存到暂存区

    已暂存(staged): 把修改后的文件放在暂存区

    已提交(committed): 把暂存区的文件提交到本地仓库

    main/master : 默认主分支
    origin      : 默认远程仓库
    HEAD        : 指向当前分支的指针
    HEAD~1      : 在当前分支对应本地仓库上一次的commit的位置(上一个版本)[HEAD^]
    HEAD~4      : 上4个版本
    FETCH_HEAD  : 用于指向最近一次fetch结果的指针
    ```

    ```
    HEAD指向当前检出分支的最后一次提价，它类似于对任何引用的指针
    HEAD可以理解为当前分支，使用"checkout/switch"，HEAD会传递到新的分支

    git show HEAD
    检查HEAD状态，显示HEAD位置
    ```

    ![Alt text](image/git_18.png)


## (2)<font color=#FFE4B5>git_operate</font>

  * 添加和提交

    ```
    git add <file>
    添加一个文件到stage
    git add .
    将所有untrack+modified文件添加到暂存区
    git add *.txt
    使用通配符指定通用文件

    已经track的文件忽略跟踪
    git update-index --assume-unchanged <file>
    恢复跟踪
    git update-index --no-assume-unchanged <file>
    查看忽略跟踪文件
    git ls-files -v | grep '^h'
    批量恢复跟踪
    git ls-files -v|grep '^h' |awk '{print $2}' |xargs git update-index --no-assume-unchanged


    git commit -m "message"
    提交所有暂存区的文件到repository

    git commit -am "message"
    git commit -a -m "message"
    提交所有已修改的文件到repository
    ```

    git commit --amend
    追加提交，在不增加一个新的commit的情况下，将新修改的代码追加到前一次commit中
    避免许多无用的提交
    --no-edit: 追加提交，且不修改message

    ![Alt text](image/git_22.png)

  * 查看工作区+暂存区修改状态

    ```
    git status
    ```

  * 查看提交日志

    ```
    git log +[option]
    * --pretty=oneline: 将提交信息显示为一行
    * --abbrev-commit: 使得输出commit更简单
    * --graph: 以图的形式显示
    * --all: 显示所有分支
    可以同时添加

    git reflog
    可以查看已经删除/回退的提交记录

    git diff
    查看未暂存的文件更新了哪些部分

    git diff <commit-id> <commit-id>
    查看2个提交之间的差异

    git diff --name-only <commit-id> <commit-id>
    仅显示2次提交文件名的差异

    git show --name-only <commit-id>
    查看某次提交中修改情况，仅显示文件名

    git diff <commit-id> -- <file-name>
    显示某次提交指定文件对应的修改详情
    e.g.
    git diff <commit-id> -- *.txt
    其中可以使用通配符指定通用文件
    ```

    ![Alt text](image/git_02.jpg)

  * 分支

    ```
    git branch
    查看所有本地分支，当前分支前面有一个星号*,
    -r：查看远程分支
    -a: 查看所有分支

    git branch <branch-name>
    基本当前HEAD创建一个新的分支

    git checkout <branch-name>
    切换分支[存在文件名与分支名相同的情况，会优先文件]

    git switch <branch-name>
    切换分支[推荐]

    git checkout <commit-id>
    以指定的节点创建一个临时性分支
    此命令比较特殊，执行此命令后，工作区内容会变成<commit-id>提交节点内容
    但是HEAD不位于任何分支上，处于游离状态
    更确切说，此命令会在<commit-id>提交节点创建一个临时分支，被别的HEAD指向这个临时分支
    你可以在这个临时分支上修改内容+提交内容
    但是你从临时分支切换到其他分支，这个临时分支就会消失
    这种临时性分支主要用于实验性的修改，实验结束，只需要切换回原分支即可，原分支不会有任何改变
    如果你想把临时分支上的改动反映到原分支，通过git checkout -b <branch-name>
    以临时性分支的当前状态创建一个永久性分支，再把这个分支合并到原分支再删除该分支即可

    git checkout -b <branch-name>
    以当前分支的当前状态创建新分支并切换到新分支
    -b: 表示创建新分支

    git checkout -b <branch-name> <commit-id>
    以当前分支<commit-id>提交节点创建新的文字并切换到新分支，此时，工作区内容和<commit-id>提交节点是本地仓库内容保持一致

    git checkout -b <branch-name> origin/<branch-name>
    在本地创建和远端分支对应的分支(本地和远程分支的名字最好一致)

    git branch -d <branch-name>
    删除一个已经合并的分支

    git branch -D <branch-name>
    删除一个分支，不管是否合并

    git push origin -delete <branch-name>
    删除远程分支

    git branch -m <old-branch-name> <new-branch-name>
    重命名分支

    git tag <tag-name>
    给当前的提交打上标签，通常用于版本发布

    git merge <branch-name>
    把<branch-name>分支合并到当前HEAD

    git rebase <branch-name>
    先寻找<branch-name>和HEAD共同的父节点，将<branch-name>改动信息变基到HEAD

    git merge --no-ff -m message <branch-name>
    合并分支
    --no-ff: 禁用Fast Forward模式，合并后的历史有分支，能查出曾经做过合并
    -ff: 表示使用Fast Forward模式，合并后历史会变成一条直线

    git squash <branch-name>
    合并&挤压(squash)所有提交到一个提交中

    git cherry-pick <commit-hash>
    将指定某次commit应用于当前分支

    ```

    ![Alt text](image/git_03.png)

    ![Alt text](image/git_04.png)

    ![Alt text](image/git_28.png)

    ```
    rebase/merge概念
    git rebase/git merge所处理是相同的问题，用于把一个分支的变更整合进另一个分支
    但是达成的目的不一样

    场景：当你开始在一个专有的分支开发功能时，另一个团队成员更新了main分支的内容
    这样就会造成一个分叉的提交历史

    example1: merge
    把main分支合并进feature
    git checkout feature    HEAD指向feature分支
    git merge main          将main分支融入feature，main分支不变化

    这会在feature分支中创建一个合并提交，这次提交会连接2个分支的提交信息
    使用merge对分支历史没有破坏性，现存的分支不会发生任何改变
    如果当feature分支需要应用上游分支的更改时，会在提交历史上增加一个无关的提交记录
    如果main分支更新非常活跃，对功能分支提交历史会产生很多污染

    example2: rebase
    把feature分支重新变基 master内容
    把main分支的提交历史变基feature分支的提交历史 底端
    git checkout feature    HEAD指向feature
    git rebase main         将master分支插入feature底部，master不变，feature基座变了，但是HEAD的指针不变

    这会把feature分支的起始历史放在main分支的最后一次提交，达到使用main分支的新代码目的
    相对于merge操作中新建一个合并请求，rebase操作会通过为原始分支的每次提交创建全新的提交，从而重写原始分支的提交历史
    使用rebase操作的最大好处可以让项目变得干净整洁
    缺点：安全性和可追溯性

    rebase黄金法则
    永远不要在公共分支上使用它[如果是本地分支rebase公共分支就没有问题]
    把feature分支rebase到main分支之下，会把所有提交放在feature分支的提交记录
    这个改变目前只会出现在你的本地仓库，其他开发者仍在原来的main分支上开发
    由于rebase产生了全新的提交记录，git认为你本地main分支与其他人产生了分叉
    唯一能够同步2个不同的main分支方式就是采用merge，这会产生一个冗余合并
    并且这次合并中大部分提交内容都是相同的
    所以在执行git rebase之前，先确认"是否有其他人使用此分支"
    ```
    ![Alt text](image/git_12.png)
    ![Alt text](image/git_13.png)
    ![Alt text](image/git_14.png)
    ![Alt text](image/git_15.png)
    ![Alt text](image/git_16.png)

  * 撤销和恢复

    ```
    git rm <file>
    从工作区+暂存区删除一个文件，并且将这次删除放入暂存区

    git rm --cached <file>
    从暂存区中删除文件，但是本地工作区文件还在

    git checkout filename
    将工作区的指定文件恢复到暂存区状态

    git checkout .
    将工作区所有文件恢复到暂存区状态

    git checkout <file> <commit-id>
    恢复一个文件到之前的版本

    git revert <commit-id>
    创建一个新的commit，用来撤销指定的提交，后者所有变化都被前者抵消，并应用到当前分支
    作用：git删除已推送到远程分支的git提交(自动创建commit|在推送之后才能生效)
    若revert一个"revert commit0(用于恢复commit0的提交)",将会还原至"commit0"的时期
    step-1: git revert <commit-id>
    step-2: git push <remote-name> <branch-name>

    git reset --mixed <commit-id>
    重置当前分支HEAD为之前的某个提交，并且删除所有之后的提交
    --hard  : 重置工作区+暂存区
    --soft  : 重置暂存区
    --mixed : 重置工作区
    e.g. 目前有三个提交C1-C2-C3，HEAD指向C3
    git reset --soft HEAD~1
    HEAD指向C2，C3的改变保存在暂存区，同理工作区也有该文件
    git reset --mixed HEAD~1
    HEAD指向C2，但是C3的改变还未add到暂存区，工作区保持不变
    git reset --hard HEAD~1
    HEAD指向C2，C3的变化不在暂存区+工作区，工作区此时直接替换为C2的commit

    git restore --stage <file>
    撤销暂存区的文件，重新放回工作区[git add的反向操作]

    git restore <file>
    重置工作区文件的修改
    ```

    ![Alt text](image/git_05.jpg)
    ![Alt text](image/git_06.png)
    ![Alt text](image/git_07.png)

  * 解决冲突

    ```
    step-1: 处理文件中冲突地方
    step-2: 将解决完冲突的文件加入暂存区
    step-3: 提交到仓库
    ```

    ![Alt text](image/git_11.jpg)

    ```
    git status -s | grep 'UU'
    显示具有未解决的冲突文件名

    使用vscode解决冲突：查看本地网页"提交代码出现冲突解决方案git用pull后冲突解决vscode.html"
    Accept Current Change（采用当前更改）：只保留当前的修改，也就是main主分支上的修改
    Accept Incoming Change（采用传入的更改）：使用reg分支上的修改
    Accept Both Changes（保留双方更改）：双方的修改都保留，这时需要我们手动解决冲突
    Compare Changes（比较变更）：比较按钮
    ```

    ![Alt text](image/git_23.png)

  * 储藏当前工作区stash

    ```
    有时候你想要切换分支，但是你正在进行当前项目一个未完成的部分
    你不想提交一半完成的工作
    git stash允许你在不提交当前分支的修改内容下切换分支

    git stash save "message"
    stash操作可以把当前工作现场"储藏"起来，等以后恢复现场后继续工作
    -u: 把所有未跟踪的文件也一并存储
    -a: 把所有未跟踪的文件+忽略的文件也一并存储
    save: 表示存储的信息

    git stash list
    查看所有stash

    git stash pop <stash-id>
    恢复指定stash-id内容
    git stash pop stash@{2}
    恢复指定的stash，stash@{2}表示第三个stash，stash@{0}表示最近的stash
    如果在应用stash时发生冲突,stash记录不会被删除,而是保留stash_list

    git stash apply <stash-id>
    重新接收指定stash-id内容
    pop与apply区别: pop会把stash内容删除，apply不会

    git stash drop stash <stash-id>
    删除指定stash-id

    git stash clear
    删除所有stash

    git stash branch <branch-name>
    如果你在特定分支储藏了一个工作并继续在该分支上工作，那么合并时可能会产生冲突
    最好在一个单独的分支上储藏你的工作
    作用：创建一个新的分支，并将储藏的工作转移到该分支
    ```

    ![Alt text](image/git_17.png)

  * 远程仓库

    ```
    git remote add <remote-name> <remote-url>
    添加远程仓库

    git remote -v
    查看远程仓库

    git remote rm <remote-name>
    删除远程仓库

    git remote rename <old-name> <new-name>
    重命名远程仓库

    git pull <remote-name> <远程branch-name>
    从远程仓库拉取代码到本地分支HEAD，默认拉取远程仓库名origin的master/main分支
    将远端仓库的修改拉到本地并自动进行合并=fetch+merge

    git pull <remote-name> <远程branch-name> : <本地branch-name>
    将远程指定分支拉取到本地指定分支

    git pull --rebase <remote-name> <远程branch-name>
    将本地改动的代码rebase到远程仓库的最新代码上[注意merge与rebase区别]

    note: pull是远程在前本地在后，push是本地在前远程在后

    git push <remote-name> <本地branch-name>
    将本地分支推送到与本地同名远程分支上

    git push <remote-name> <本地branch-name> : <远程branch-name>
    将本地分支推送到远程指定分支上

    git push origin HEAD: <远程branch-name>
    显式指定推送HEAD到远程仓库中的分支

    git fetch <remote-name>
    获取所有远程分支

    git fetch <remote-name> <branch-name>
    fetch某一特定的远程分支
    ```

    ![Alt text](image/git_19.png)
    ![Alt text](image/git_08.jpg)
    ![Alt text](image/git_09.jpg)
    ![Alt text](image/git_10.png)

## (3)<font color=#FFE4B5>git_bug</font>

  * git合并bug

    * 问题1

    ```
    merge brach "dev"
    # Please enter a commit message to explain why this merge is necessary,
    # especially if it merges an updated upstream into a topic branch.
    #
    # Lines starting with '#' will be ignored, and an empty message aborts
    # the commit.

    翻译:
    # 请输入提交信息，解释为什么必须合并、
    # 尤其是在将更新的上游分支合并到主题分支的情况下。
    #
    # 以 "#"开头的行将被忽略，空信息将终止
    # 提交。

    原因:
    在软件开发过程中，通常会有多个开发人员同时修改远程仓库中的不同部分，当这些修改需要合并到远程分支
    要求用户输入提交信息来解释为什么采取这次合并
    这是由于合并操作可能导致潜在的冲突和问题，为了避免混乱和误解，清晰明确的提交信息时必要的

    思路:
    step-1:
    :wq
    step-2:
    exit+保存
    ```

    * 问题2

    ```
    git rebase --continue
    会显示:
    No changes - did you forget to use 'git add'?
    If there is nothing left to stage, chances are that something else
    already introduced the same changes; you might want to skip this patch.

    When you have resolved this problem, run "git rebase --continue".
    If you prefer to skip this patch, run "git rebase --skip" instead.
    To check out the original branch and stop rebasing, run "git rebase --abort"

    翻译:
    没有改动，你是不是忘了使用"git add"？
    如果没有任何剩余改动，那么可能是其他东西已经引入相同的改动，你可能想跳过这个补丁
    问题解决后，运行"git rebase --continue"即可
    如果你想跳过这个补丁，请运行"git rebase --skip"代替它
    要退出原始分支并停止重定向，请运行"git rebase --abort"

    原因:
    即便是已经处理了冲突，但是这个冲突选择以Local作为处理结果，需要跳过之前git的对之前文件的修改

    思路:
    这个时候需要通过 git rebase --skip 来跳过
    ```

    * 问题3

    ```
    error: src refspec xxx does not match any / error: failed to push some refs to

    原因:
    git push <remote-name> <branch-name远程分支名>
    当将本地分支push到远端同名分支时，branch-name只需要写一个分支名就可以

    思路:
    当要push到远端分支名不同于本地分支名
    git push <remote-name> [本地分支名:远端分支名(不要有空格)]
    ```

 * git配置bug

    * 问题1

    ```
    ”git init – bad numeric config value “auto” for “core.autocrlf” in

    翻译:
    git初始化中，对于参数"core.autocrlf"设置了错误的数值"auto"

    原因:
    参数"core.autocrlf"配置错误

    思路:
    step-1:
    git config --global --list
    列出全局配置的键值对，找到"core.autocrlf"对应的行
    step-2:
    git config --gloabal core.autocrlf true/input/false
    ```

    * 问题2

    ```
    解决新环境下git status显示一大堆modified文件，文件权限问题：old mode 100755；new mode 100644

    原因:
    linux/win/mac之间文件模式不同导致的

    思路:
    git config core.filemode false // [在本地指定项目配置]
    git config --global core.filemode false // [全局配置，一般没啥效果]
    ```

  * git推送bug

    * 问题1

    ```
    新建本地仓库master分支后，你在提交了修改内容直接进行push首次会出现失败

    原因: 我们只是在本地建立一个仓库，没有将本地仓库与远程仓库进行关联

    思路:
    git remote add origin <repo_address>
    本地仓库关联远程仓库

    如果本地仓库通过git clone，git会自动帮你关联远程仓库
    ```

    ![Alt text](image/git_20.png)

    * 问题2

    ```
    fatal: The upstream branch of your current branch does not match
    the name of your current branch. To push to the upstream branch on the remote

    翻译:
    上游分支名[远程分支名]与你本地分支名不匹配

    原因:
    git push是没有指定参数是默认推送到与其建立跟踪关系的远程分支
    如果没有建立跟踪关机，就会失败

    思路:
    1、将本地分支名与远程分支名一样即可

    2、本地分支与远程分支建立跟踪[track]关系
    git push -u <remote-name> <本地branch-name> : <远程branch-name>
    或者
    git branch --set-upstream-to=<remote-name>/<远程branch-name> <本地branch-name>
    ```

    ![Alt text](image/git_21.png)

    * 问题3

    ```
    hint: Updates were rejected because the tip of your current branch is behind
    hint: its remote counterpart. If you want to integrate the remote changes,
    hint: use 'git pull' before pushing again.
    hint: See the 'Note about fast-forwards' in 'git push --help' for details.

    翻译:
    更新被拒绝，因为当前分支的提示位于其后面

    原因:
    你的当前分支的最新提交落后于远程分支的最新提交。这通常发生在你试图将本地分支的更改推送到远程分支时。在本地仓库上的修改没有基于远程库最新版本，你的本地仓库版本落后于远程仓库。为了解决这个问题，你需要先将远程分支的更改合并到本地分支，然后再次尝试推送你的更改

    思路:
    1、强制推送[如果确定没人推送你的分支]
    git push -f origin master:master
    git push --force origin master:master
    git push origin master:master -f
    git push origin master:master --force

    2、拉取一次最新分支代码
    pull --rebase
    ```

    * 问题4

    ```
    hint: Updates were rejected because the remote contains work that you do not
    hint: have locally. This is usually caused by another repository pushing to
    hint: the same ref. If you want to integrate the remote changes, use
    hint: 'git pull' before pushing again.
    hint: See the 'Note about fast-forwards' in 'git push --help' for details.

    翻译:
    远程仓库的部分东西本地仓库没有

    思路:
    pull --rebase // [拉取一次最新分支代码]
    ```

  * git克隆bug

    * 问题1

    ```
    The authenticity of host 'github.com (20.205.243.166)' can't be established.
    RSA key fingerprint is SHA256:xxxxxxxxxxxxxxxxxx.
    This key is not known by any other names
    Are you sure you want to continue connecting (yes/no/[fingerprint])?

    翻译:
    github.com的真实性无法成立

    原因:
    新电脑中host没有关于github内容

    思路:
    不用管，直接回车，根据提示再写入yes，回车
    ```
