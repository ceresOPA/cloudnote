# git

?> 学习网站：https://learngitbranching.js.org/?locale=zh_CN

## git基础

### 1. git commit(提交记录)

通过git commit可以提交一个记录，记录只是当前目录文件的一个快照。每次提交记录，都会与上一次的记录比较，只提交有差异的部分。

### 2. git branch(创建分支)

通过git branch 分支名称，可以创建一个新的分支，建立分支也只是增加一个对某记录的指向。

可以通过 **git checkout 分支名称**来切换分支（貌似新的指令是 git switch ，还没怎么去了解，写学基础）

创建分支并切换到该分支：**git checkout -b 新分支名称**

### 3. 合并分支

#### git merge

不同的分支有可能是在实现不同的功能部分，当完整整个功能后，希望能够合并为一份完整的代码，则可以利用git merge来实现。

```
git checkout main //切换到main分支下
git merge version1 //把version1分支合并到main分支下
```

#### git rebase

使用git rebase可以实现一个更加线性的提交记录

```
git checkout branch1 // 进入branch1分支
git rebase main //把branch1合并到main分支下
//通过rebase后，会认为branch1只是main分支的一个新的提交记录，是线性提交的一个过程
```

### 4. git checkout 切换分支

在已知各记录的哈希值的情况下，可以通过**git checkout 哈希值** 进行分支的切换，即HEAD指向的记录的变更，并没有改变分支的指向。除了通过哈希值进行切换，还可以通过相对位置进行切换。

```
git checkout hds // 这里不一定要把所有的哈希值都写出来，只要能够唯一的区分出记录就可以了, 可以通过 git log 来查看哈希值
// 通过上面的操作后，HEAD就会指向哈希值为hds的记录
// 相对路径
git checkout HEAD^ //移动到当前记录的父结点
git checkout HEAD~3 //移动到父结点的父结点的父结点，就是前移3个，到第3个为止
```

这里稍微注意一下HEAD是大写吧，因为我之前使用小写head的时候，说找不到，所以应该是区分大小写的。

### 5. git branch -f 修改分支

通过 git branch -f 分支名 切换位置 可以将分支切换到指定的记录处

```
git branch -f main c0 //可以将main分支切换到c0对应的记录处
git branch -f main main~3 // 可以将main分支前移3个记录 ，可以类比git checkout的切换，git checkout只动HEAD，不动分支
git branch -f main HEAD^ //切换到HEAD的父结点
```

### 6. 撤销记录

#### git reset(本地)

本地撤销记录，直接将分支指向前移到指定记录即可。

```
git reset main~2
// 直接前移，恢复到之前的记录
```



#### git revert(远程)

```
git revert c1
// 指的是撤销c1的修改，会在当前记录后新提交一个记录，专门用于撤销c1记录的修改
```



?> 这里稍微注意一下git reset 和 git revert 后面加的参数的区别，git reset 后面是 移动到指定位置，有点像git branch -f。而git revert 后面是指撤销的记录是哪个。







## git使用中遇到的问题

### OpenSSL SSL_read: Connection was reset, errno 10054

?> 把笔记上传到GitHub的时候，冒出了这个错误，原本还以为是网络问题，然后搜了一下，说是因为**服务器的SSL证书没有经过第三方机构的签署导致错误**。所以，重新全局配置一下就好了：

```shell
git config --global http.sslVerify "false"
```

### ERROR: 'COMMIT' IS NOT POSSIBLE BECAUSE YOU HAVE UNMERGED FILES.

解决方案：

1. 把修改的文件add下，如：git add bidder_mod/src/common/dragon_bidder_data.cc
2. git commit