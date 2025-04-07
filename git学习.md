# <font face="仿宋" font color=black>Git学习笔记</font>
## 一、本地仓库
1. **仓库是什么**
仓库表示受git版本控制的一个项目目录，可分为本地仓库（Local Repository）与远程仓库（Remote Repository）。
2. **初始化本地仓库**
   ```
   git init //初始化一个本地仓库
   git init -b <branch_name> //初始化一个本地仓库，同时将初始分支设为<branch_name>
   ```
3. **Git工作区**
   [![pEcD4aT.png](https://s21.ax1x.com/2025/04/07/pEcD4aT.png)](https://imgse.com/i/pEcD4aT)
   1. **工作目录**
        即为项目文件/目录编辑区。
   2. **本地仓库**
   3. **暂存区**
        草稿区，可以添加或删除文件，由index文件表示，在暂存区至少添加一个文件时才会创建index文件。
   4. **提交历史区**
        1. **什么是提交（commit）？**
            提交是该项目的一个版本/快照，包含针对这个提交的所有文件引用，对应一个唯一的commit hash
        2. 提交历史区即为保存所有提交历史的地方，保存于objects目录中
4. **未跟踪的文件（untracked file）与跟踪的文件（tracked file）**
     1. 未跟踪的文件指在工作目录中，未受Git版本控制的文件，不在暂存区与提交历史中
     2. 跟踪的文件则指被添加到暂存区并进入提交历史的文件
5. **实操过程记录**
   使用git init进行本地仓库初始化
   [![pEcDsPg.png](https://s21.ax1x.com/2025/04/07/pEcDsPg.png)](https://imgse.com/i/pEcDsPg)
## 二、提交
_Commit early，commit often_
1. **为什么要提交？**
     避免成果丢失
2. **提交流程**
   1. 将需要提交的文件添加到暂存区
   2. 提交文件+提交消息
3. **将文件添加到暂存区相关命令**
   ```
   git add <filename> //将一个文件添加到暂存区
   git add <filename> <filename> //将多个文件添加到暂存区
   git add -A //将多个修改过的文件添加到暂存区，整个项目
   git add . //将多个修改过的文件添加到暂存区，当前目录和子目录
   ```
4. **提交命令**
   ```
   git commit -m "<message>" //创建一个新提交+提交信息
   ```
5. **操作后工作区变化**
   暂存区中会增加一份本次操作文件的拷贝，提交历史区中生成一段对应的commit hash值。
6. **查看提交列表命令**
   ```
   git log //展示一系列提交信息，包括commit hash值，作者名称和email地址，提交时间，提交信息与HEAD
   git log --all //展示本地仓库中所有分支上的提交
   ```
7. **实操过程记录**
   1. 首先在工作目录中新建一个1st.txt文件，文件名旁边的U即为Untracked，因还未添加到暂存区导致。
      [![pEcr9RH.png](https://s21.ax1x.com/2025/04/07/pEcr9RH.png)](https://imgse.com/i/pEcr9RH)
   2. 通过git add将该文件加入暂存区中
      [![pEcrpJe.png](https://s21.ax1x.com/2025/04/07/pEcrpJe.png)](https://imgse.com/i/pEcrpJe)
   3. 使用git commit进行提交并使用git log查看提交记录
      [![pEcrCzd.png](https://s21.ax1x.com/2025/04/07/pEcrCzd.png)](https://imgse.com/i/pEcrCzd)
## 三、分支
 _分支是指向commit的可移动指针_
1. **什么是分支？**
   用于支持多人合作开发的多条开发线，每个分支表示项目的一个独立版本。
   [![pEcrZo8.png](https://s21.ax1x.com/2025/04/07/pEcrZo8.png)](https://imgse.com/i/pEcrZo8)
   .git文件夹中heads文件中的文件表示该项目的所有本地分支，文件内容为对其最新提交的引用
   [![pEcrFsI.png](https://s21.ax1x.com/2025/04/07/pEcrFsI.png)](https://imgse.com/i/pEcrFsI)
2. 工作目录中已被Git跟踪的文件可以有两种状态
   * Unmodified files：自上次提交后未被修改过的文件
   * Modified files：自上次提交后又被修改的文件
3. **分支相关命令**
   ```
   git branch //列出本地分支
   git branch <new_branch_name> //新建一个分支
   git branch --all //列出
   git switch <branch_name> //切换到对应分支
   ```
4. **什么是HEAD**
   HEAD就是指向当前分支的指针，可以通过git log或git branch查看
   ![](https://pic1.imgdb.cn/item/67f33d8e0ba3d5a1d7ef092f.png)
5. **切换分支的具体步骤**
   1. 将HEAD指向目标分支
   2. 使用和目标分支相对应的提交快照填充暂存区
   3. 将暂存区内容拷贝到工作目录
6. **实操过程记录**
   1. 首先新建一个名为feature的分支，并使用git branch查看本地分支情况
      ![](https://pic1.imgdb.cn/item/67f3405a0ba3d5a1d7ef0a9d.png)
   2. 使用git switch切换到对应分支
      ![](https://pic1.imgdb.cn/item/67f340720ba3d5a1d7ef0abc.png)
   3. 观察到切换后.git文件夹中HEAD内容发生变化
      ![](https://pic1.imgdb.cn/item/67f340700ba3d5a1d7ef0ab3.png)
## 四、合并
1. **什么是合并**
   合并就是将一个分支的变更集成到另外一个分支去。
2. **两种合并方式**
   * 快进合并
   * 三方合并
  二者的区别在于两个分支的开发历史是否出现了分叉
3. **合并相关命令**
   ```
   git merge <branch_name> //将源分支的变更集成到目标分支中
   ```
4. **实操过程记录**
   1. 在feature分支中对1st.txt文件进行修改，M表示modified
      ![](https://pic1.imgdb.cn/item/67f341c00ba3d5a1d7ef0b67.png)
      ![](https://pic1.imgdb.cn/item/67f3421a0ba3d5a1d7ef0ba5.png)
   2. 在feature分支中将修改过的1st.txt文件进行提交
      ![](https://pic1.imgdb.cn/item/67f342f60ba3d5a1d7ef0c14.png)
   3. 切换至master分支，并进行合并
      ![](https://pic1.imgdb.cn/item/67f3438b0ba3d5a1d7ef0c4b.png)
## 五、远程仓库
1. **使用Git开始项目工作的方式**
   * 从本地仓库开始
      1. 创建本地仓库
      2. 创建远程仓库并将数据推送到远程仓库
      3. 基于本地和远程仓库开展工作
   * 从远程仓库开始
      1. 创建或找到一个远程仓库
      2. 将远程仓库克隆到本地
      3. 基于本地和远程仓库开始工作
2. **在本地与远程仓库建立连接命令**
   ```
   git remote add <shortname> <URL> //将本地仓库连接到远程仓库，连接名<shortname>，远程仓库地址<URL>
   git remote // 列出本地到远程仓库的所有连接的连接名
   git remote -v // 列出本地到远程仓库的所有连接的连接名与URL
   ```
3. **分支的种类**
   * 本地分支即为本地仓库中的分支
   * 远程分支为在远程仓库中的分支，本地分支变化时，远程分支并不会自动变化。
   * 远程跟踪分支为在本地仓库中的对远程分支指向提交的一个引用
   * 上游分支为本地与远程分支之间建立的跟踪关系，上游分支是本地分支指向的远程分支
4. **将本地仓库内容推送至远程仓库命令**
   ```
   git push <shortname> <branch_name> //将本地分支<branch_name>中的内容推送至<shortname>中对应的远程分支中
   git push -u <shortname> <branch_name> //设定上游分支
   git branch --all //列出所有本地分支与远程跟踪分支
   ```
5. **实操过程记录**
   1. 利用git remote add建立本地与远程仓库连接
   2. 将本地仓库内容推送至远程仓库中
## 六、克隆与拉取
1. **克隆命令**
   ```
   git clont <URL> <directory_name> 
   ```
   实际过程为：
   1. 在当前目录中创建一个项目目录
   2. 初始化一个本地仓库
   3. 从远程仓库下载所有数据
   4. 在本地仓库与远程仓库之间建立一个连接
2. **关于删除分支**
   1. 删除分支的目的是为了保持项目结构清晰
   2. 删除分支前，需要确保该分支上的新内容已经合并至另外一个分支上或已经不需要该分支上的内容
   3. 要完全删除一个分支，需要同时删除：
   * 远程分支
   * 远程跟踪分支
   * 本地分支
   1. **删除分支相关命令**
   ```
   git push <shortname> -d <branch_name> //删除一个远程分支与其对应的远程跟踪分支
   git branch -d <branch_name> //删除一个本地分支
   ```
3. **查看本地和上游分支关系的命令**
   ```
   git branch -vv //列出本地分支是否定义了上游分支，并同时显示本地相对于上游分支的情况（提前/落后）
   ```
4. **将远程分支变更集成到本地步骤**
   1. 从远程分支拉取变更
   2. 将变更集成到本地分支
5. **拉取相关命令**
   ```
   git fetch <shortname> //从和<shortname>对应的远程仓库拉取数据
   ```
   tips：git fetch只影响远程跟踪分支，并不影响本地分支（不自动集成进本地）
6. **集成到本地分支**
   * 两种集成方式
      * 合并
      * 变基
   * 两种合并方式
      * 快进合并
      * 三路合并
7. **删除远程跟踪分支命令**
   ```
   git fetch -p //删除本地的远程跟踪分支
   ```
8. **实操过程记录**
   1. 在本地新建一个clone工作目录，从远程仓库将内容克隆至本地
   2. 在该工作目录对项目文件进行修改：修改1st.txt文件并删除feature分支
   3. 将改动推送至远程仓库
   4. 切换回原本的工作目录，从远程仓库中拉取变更，并删除多余分支
## 七、三方合并
1. **pull拉取命令**
   ```
   git pull <shortname> <branch_name> //从和shortname对应的远程仓库的branch_name
   分支中，拉取变更并合并入本地当前分支
   ```
2. **fetch和pull该如何选择**
   如果本地和远程分支没有分叉，则使用git pull进行快进合并
   否则先使用git fetch，然后视情况做合并或变基
   ![](https://pic1.imgdb.cn/item/67f33db90ba3d5a1d7ef0936.png)
3. **实操过程记录**
   1. 在原工作目录中新建一个2nd.txt文件，将其提交，并推送至远程仓库
   2. 在clone工作目录中对1st.txt文件进行修改，试图提交时失败，由于远程仓库与clone仓库产生分叉
   3. 在clone工作仓库使用git fetch将远程仓库内容拉取，再利用git merge实现合并
   4. 合并成功后，将clone内容推送至远程仓库
   5. 切换至原工作目录，将远程仓库内容拉取下来并合并
## 八、合并冲突
_为避免合并冲突，尽早合并_
1. **合并冲突发生的原因**
   * 两人对同一个或多个文件的相同部分进行了修改
   * 一人删除了文件，另一人修改了同一文件
2. **解决合并冲突流程**
   1. 对产生冲突的文件进行编辑并保存
   2. 将编辑好的文件添加至暂存区并提交
3. **实操过程记录**
   1. 在原工作目录中对1st.txt文件进行修改并提交推送至远程仓库
   2. 切换至clone目录，也对1st.txt文件进行不同于第一步的修改，并进行提交，从而与远程仓库产生分叉
   3. 在clone目录拉取远程变更，对1st.txt进行手动修改解决冲突后合并
   4. 将clone目录内容推送至远程仓库
   5. 原工作目录拉取远程仓库内容以获取更新
## 九、变基
1. **什么是变基**
有些团队或个人希望维持线性的项目提交历史已保证项目过程清晰，变基就可以维持线性提交历史，但变基会产生新提交，改变提交历史
1. **变基命令**
   ```
   git rebase <branch_name> //在另一个分支上重新应用当前分支上的新提交
   git rebase --continue //解决冲突后继续变基过程
   git rebase --abort //撤销并回到变基前的状态
   ```
2. **变基过程**
   1. 找到共同祖先
   2. 暂存要变基的分支信息
   3. 重置HEAD
   4. 应用+提交变更
   5. 切换到变基后的分支
3. 变基时遇到冲突时需要人工介入，且可能会有多次冲突。
4. **实操过程记录**
   1. 在原工作目录中修改2nd.txt文件，并推送至远程仓库中
   2. 在clone目录中修改1st.txt与2nd.txt文件，并提交
   3. 在clone目录中拉取远程仓库变更，使用git rebase进行变基，并解决冲突
   4. 将变基后成果推送至远程仓库中，切换至原工作目录拉取以获取更新
## 十、合并请求
1. **什么是合并请求**
   合并请求（Pull Request）是一种在代码托管平台上提交代码的方式，方便你将分支上的工作分享给协作者，收集反馈，并最终将代码合并到项目中。
2. **Pull Requst流程**
   1. 在本地创建分支
   2. 在本地分支上工作并提交
   3. Push到远程仓库
   4. 在远程代码托管平台上创建pull request
   5. 请求评审并收集反馈
   6. 评审通过则合并请求
   7. 删除远程分支
   8. 在本地通过拉取合并同步远程仓库，并删除本地分支与远程跟踪分支
   