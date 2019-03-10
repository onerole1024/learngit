我们做毕业设计一般包含代码部分和论文，为了保存论文每次更改记录，会复制重命名，
对于代码部分一般也会复制文件夹记录版本。在企业当中是一个团队在开发，显然需要
版本管理，能够查看每次提交的代码比上个版本的变化，如果版本存在问题，可以回滚到
老版本等等。此处介绍企业中都在用的git，工作流程：
本地工作区【代码目录】-》到暂存区【加入git文件跟踪,文件索引】-》到本地仓库-》
远程仓库【大家都可以访问的服务器，例如github】

一、git 下载：https://git-scm.com/downloads

二、git本地仓库创建，创建文件，添加到暂存区，提交到本地仓库

cd  /Users/free/learngit     //到工作目录
git  init     //初始化版本库
echo "第一个版本代码xxxx" >> test.txt   //追加这段文字到test.txt文件
git status   //查看工作区状态 以下返回内容
On branch master
No commits yet
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	test.txt
nothing added to commit but untracked files present (use "git add" to track)·

git add test.txt  //将test.txt添加到暂存区

//提交到本地仓库    "第一个版本" 为提交备注，方便日后查看这次提交代码的主要功能
git commit -m "第一个版本"   

三、将本地仓库推送到github或者其他远程仓库中，与其他人共同开发

//接下来设置账户，跟github用一个账户，并推送本地仓库到github这个远程仓库上
//--global  如果加这个就会设置全局，不加则只设置当前仓库所用账户
cd .git  //要进入项目目录下的.git文件夹 进行账户设置
git  config  [--global] user.name "onerole1024"
git  config  [--global] user.email "onerole@126.com"
git config --list   //查看配置
cat config  //查看配置
//本地仓库关联到远程仓库，远程仓库的名称一般默认为 origin，也可以设置为其他名称
git remote add origin https://github.com/onerole1024/learngit.git
 //将本地仓库master分支推送到origin主机，指定origin为默认主机，以后可以不加任何参数使用git push
git push -u origin master 
cd .. //从.git 目录到上层目录
echo "第二个版本">>test.txt    //追加这句话到test.txt文件
git add test.txt  //将test.txt添加到暂存区
git commit -m "第二个版本"    //提交到本地仓库
git push  //将本地仓库推送到远程github，此时查看github就会看到提交记录
git pull    //拉取远程服务器上最新代码

四、将变动提交到暂存区，还未提交到本地仓库，但变动不想要了，需要丢去

echo "第二个版本的细微改动" >>test.txt         //对代码进行修改
git add test.txt         //添加到暂存区
git reset HEAD test.txt       //后来发现这个小功能不想要了，需要丢弃
git checkout -- test.txt      //将工作区变为暂存区的状态

五、将变动已经提交到本地仓库，但变动不想要了，需要回滚到上一个版本

echo "第二个版本增加扩展功能" >>test.txt         //对代码进行修改
git add test.txt         //添加到暂存区
git commit -m "第二个版本扩展功能"    //提交到本地仓库
git  log        //查看提交记录，以下是提交记录
commit 049ffa85228bdc65ec9c3061c1438e36478ae007 (HEAD -> master)
Author: onerole1024 <onerole@126.com>
Date:   Sat Mar 9 21:21:47 2019 +0800
    第二个版本扩展功能

commit 52c576e19eef9bcd7e3354598d12d242419621c1 (origin/master)
Author: onerole1024 <onerole@126.com>
Date:   Sat Mar 9 20:28:14 2019 +0800
    第二次提交

commit e4338a16c7d1e16be2ea003fe7d2f875e687c64f
Author: onerole1024 <onerole@126.com>
Date:   Sat Mar 9 20:24:08 2019 +0800
    第一次提交

//将仓库和暂存区都回滚到上次提交的版本，复制上次提交的版本号，执行回滚
git reset --hard  52c576e19eef9bcd7e3354598d12d242419621c1
//输出：HEAD is now at 52c576e 第二次提交

**********git reset --hard 之后，git log就看不到放弃的提交了，git reflog则可以看到被删除的commit
如果后悔，则可以再 git reset --hard xxx    恢复到某删除的版本

六、项目增加了一个功能，也提交到了本地仓库，但完全不需要了，需要删除这个功能

echo "增加人脸识别功能" >> face.txt
git add face.txt 
git commit -m "人脸识别"
git rm face.txt    //删除变动
git status  //查看状态，需要将删除提交到本地仓库
//输出：
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	deleted:    face.txt
git commit -m "丢弃人脸识别功能"     //将删除提交到本地仓库

七、项目增加某功能提交到本地仓库，扩展功能也提交到暂存区，但扩展功能不需要了

echo "发送语音" >>yy.txt
git add yy.txt
git commit -m "语音功能"
echo "语音识别" >>yy.txt
git add yy.txt
git checkout HEAD yy.txt      //将版本库的文件覆盖表暂存区和工作区


八、项目已经提交到远程服务器，但是该功能不需要了，将远程本地回滚到上个版本

echo "增加表情功能" >> bq.txt
git add bq.txt 
git commit -m "表情功能"
git push
git revert HEAD  //回滚到上个版本  与reset --hard 的区别就是会生成一次提交
git commit -m "不需要发送表情"  
git push


九、总结工作区、暂存区、本地仓库工作流、远程仓库

工作区改动 通过add提交到暂存区，通过commit提交到本地仓库，
push推送到远程仓库。
reset HEAD 会将本地仓库某文件最新版本覆盖掉暂存区文件，checkout -- 
会将暂存区文件覆盖掉工作区文件。checkout HEAD 会将本地仓库某文件
最新版本覆盖掉暂存区和工作区文件。如果需要将本地仓库回滚到之前某个版本
则git reset --hard  版本号，这样本地仓库和暂存区指针都会指向这个版本，
这个版本号的文件会覆盖掉工作区文件。
git push origin master -f  ,-f 参数是强制提交，因为reset之后本地库落后于
远程库版本，因此需要强制提交。
git revert HEAD  和 reset 的区别是，前者会保留要放弃的提交记录。
暂存区index 和 本地仓库HEAD 都是指针形式指向objects文件hash。



十、标签管理,每发个版本打一个标签，便于管理，每一个标签就像一个里程碑式发布

git tag v2.0   //打一个标签   v2.0  
git tag           //查看所有打的标签
git push origin v2.0  //将标签推送到远程仓库【远程仓库默认命名origin】
*********以上操作会将远程仓库当前的代码打一个标签，该标签如同当前代码内容的******* 
echo "增加方言识别" >>  yy.txt 
git add yy.txt 
git commit -m "方言"
git push origin v2.1
echo "增加泰语" >> yy.txt 
git add yy.txt 
git commit -m "泰语"
git tag v2.2
git push origin v2.2
git tag -d v2.2  //删除本地仓库标签
git push origin :refs/tags/v2.2  //删除远程该标签     git push origin :refs/tags/标签名
****** 以上命令把修改代码本地打标签并推送到远程相应的标签，并未到远程主分支****** 
git push  //将变动推送到主分支

*****如果某一个发布的标签出现bug，可以查看该标签对应的commit提交号，将
本地仓库版本回滚到该版本，然后拉取分支，将本地仓库版本再恢复到最新版本，
在分支修复bug后，本地打标签并push到远程该标签，合并代码到本地仓库主分支再push


十一、 主分支还在开发，当需要开发一个功能，与主分支不能同时发版等，可以创建分支

echo "增加人机对话功能，中文">> speak.txt
git add *  //当前修改全部加入暂存区
git commit -m "人机对话-中文"
git push
git branch feature_en       //打一个分支名为  feature_en   用做开发人机回话-英语
git branch                             //查看当前所有分支  签名带*号的为当前所在分支
git checkout feature_en  //切换到 feature_en 这个分支
echo "增加人机对话功能，英文">> speak.txt       //在分支上开发新功能
git add speak.txt 
git commit -m "人机对话-英语"
git push --set-upstream origin feature_en    //在远程仓库github上创建分支，并将修改push到远程分支
echo "增加人机对话功能，修复英文bug">> speak.txt
git add speak.txt
git commit -m "修复英文bug"
git push
git checkout master     //切换到主分支
echo "人机对话智能性增强">>speak.txt    //主分支开发了新内容
git add speak.txt 
git commit -m "智能增强"
git push         //推送到远程仓库
git tag v2.2   //创建本地仓库标签
git push origin v2.2   //推送本地标签内容到远程仓库，并建立远程仓库标签
git merge feature_en   //将分支内容合并到主分支上，由于同时都在开发，产生冲突，需要解决
cat   speak.txt 
增加人机对话功能，中文
<<<<<<< HEAD
人机对话智能性增强
=======
增加人机对话功能，英文
增加人机对话功能，修复英文bug
>>>>>>> feature_en

git add speak.txt   //解决冲突后，添加到暂存区
git commit -m "合并分支上英语对话功能"
git push






















sourcetree
git status
git add test.txt
git commit -m "submit 添加发表情功能"
git reset --hard xxx[版本号]

git add test.txt  在暂存区 还未提交到仓库
git reset HEAD  test.txt  暂存区回滚为仓库版本
git checkout -- text.txt 当前工作区变更

 
git rm test.txt   但暂存区和仓库还没有清空
git commit -m "删除"



