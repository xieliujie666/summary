

## 1.你是如何学习[前端](https://www.nowcoder.com/jump/super-jump/word?word=%E5%89%8D%E7%AB%AF)的? 说一说你的学习经历 



## 2.自己做的[项目](https://www.nowcoder.com/jump/super-jump/word?word=%E9%A1%B9%E7%9B%AE)中有没有组件封装，有没有落地应用？

组件的封装里面都做了什么，是自己用还是打包到npm了？

1.在使用 vue-cli 创建的项目中，组件的创建非常方便，只需要新建一个 .vue 文件，然后在 template 中写好 HTML 代码，一个简单的组件就完成了 一个完整的组件，除了 template 以外，还有 script和 style 

## 3.git常用命令？？？

## git merge/rebase作用 

1、git rebase 和 git merge 一样都是用于从一个分支获取并且合并到当前分支.
假设一个场景,就是我们开发的[feature/todo]分支要合并到 master 主分支

![1616770638721](C:\Users\superssssss\Desktop\Interview\mj-images\1616770638721.png)

marge 特点：自动创建一个新的 commit 如果合并的时候遇到冲突，仅需
要修改后重新 commit
o 优点：记录了真实的 commit 情况，包括每个分支的详情
o 缺点：因为每次 merge 会自动产生一个 merge commit，所以在使用一些
git 的 GUI tools，特别是 commit 比较频繁时，看到分支很杂乱。

rebase 特点：会合并之前的 commit 历史
o 优点：得到更简洁的项目历史，去掉了 merge commit
o 缺点：如果合并出现代码问题不容易定位，因为 re-write 了 history
因此,当需要保留详细的合并信息的时候建议使用 git merge，特别是需要将分支合并进入master 分支时；当发现自己修改某个功能时，频繁进行了 git commit 提交时，发现其实过多的提交信息没有必要时，可以尝试 git rebase.

2、git reset、git revert 和 git checkout 的共同点：用来撤销代码仓库中的某些更改。
然后是不同点：
首先，从 commit 层面来说：
o git reset 可以将一个分支的末端指向之前的一个 commit。然后再下次 git
执行垃圾回收的时候，会把这个 commit 之后的 commit 都扔掉。git reset
还支持三种标记，用来标记 reset 指令影响的范围：
 --mixed：会影响到暂存区和历史记录区。也是默认选项
 --soft：只影响历史记录区
 --hard：影响工作区、暂存区和历史记录区
注意：因为 git reset 是直接删除 commit 记录，从而会影响到其他开发人员的分
支，所以不要在公共分支（比如 develop）做这个操作。
 git checkout 可以将 HEAD 移到一个新的分支，并更新工作目录。
因为可能会覆盖本地的修改，所以执行这个指令之前，你需要
stash 或者 commit 暂存区和工作区的更改。
o git revert 和 git reset 的目的是一样的，但是做法不同，它会以创建新的
commit 的方式来撤销 commit，这样能保留之前的 commit 历史，比较安
全。另外，同样因为可能会覆盖本地的修改，所以执行这个指令之前，
你需要 stash 或者 commit 暂存区和工作区的更改。
然后，从文件层面来说：
o git reset 只是把文件从历史记录区拿到暂存区，不影响工作区的内容，而
且不支持 --mixed、--soft 和 --hard。
o git checkout 则是把文件从历史记录拿到工作区，不影响暂存区的内容。
o git revert 不支持文件层面的操作。



## Webpack

有试过webpack从0开始搭建 









## 反问：

1、蚂蚁技术栈是什么？

2、node是否是一定要掌握的呢？

3、对我有什么评价和建议？