# Com的财富

## 目录
1. [android 学习笔记](#android学习笔记)
2. [Markdown 学习笔记](#Markdown)
1. [git 学习笔记](#git学习笔记)

## android学习笔记 
### 1.RecycleView使用心得

  RecycleView 基本上能完全代替Listview和GridView，最重要的是RecycleView的可扩展性是另外两个控件无法比拟的
  
#### 优势

> 优点1：灵活的adapter，可以设置item的类型，根据不同的类型设置不同布局、不同的逻辑等等，轻松实现类似相册那种带着header日期的模式

> 优点2：利用setItemAnimator、addItemDecoration可以轻松定制动画和间距（装饰）等等

> 优点3：setSpanSizeLookup这个是设置item的所占大小，利用item的类型属性，可以定制出有趣的布局，比如title属性占4个位置（每一行恰好的四个），那么就能实  现title占一行

#### 踩坑点：
> 坑1：当recycleView被scollView包裹时，则失去了其加载的优势，变成了一下子把recycleview全部加载出来（猜测可能是因为scollView需要计算出高度，所以要全  部加载出来）

> 坑2：在adapter中尽量使用notifyDataSetChanged()，因为本身recycleView就是目前显示那个才去加载哪里，所以即使通知全部数据变化了，实际上也就是刷新了当   前显示的item，用其他notify可能会出问题，尝试后即可知道

> 坑3：在onBindViewHolder中，任何if都要连带着else，否则在随着列表滑动刷新的时候，因为没有else，所以原来的状态不会改变过来

> 坑4：recycleView中每个item的中的控件点击事件，如果点击完牵扯到UI刷新，建议直接调用notifyDataSetChanged()，而不是直接改变该item下的UI，防止         recycleView中数据插入与删除导致的各种不可理解现象

## Markdown
> 1.[快速入门](http://wowubuntu.com/markdown/basic.html)

> 2.[完整入门](http://wowubuntu.com/markdown/index.html)

## git学习笔记
### git命令
* git init 初始化git仓库
* git add readme.txt  或者git add.
* git commit -m "first commit"  提交到本地仓库
* git log 查看提交日志
* git log --pretty=oneline   精简日志
* git reset --hard HEAD^  回退到上一个版本   HEAD表示当前版本 HEAD^表示上一个版本，HEAD^^上两个版本，多个则HEAD~100
  或者git reset --hard 508930d     508930d为commit id
  
  git reset HEAD readme.txt  另外reset命令也可以把暂存区的修改回退到工作区
* git reflog 查看之前的命令日志，防止重启后还能吃后悔药，如果恢复到未来的版本（已经回退过，所以有未来）
* git status 查看当前状态
* git diff HEAD -- readme.txt命令可以查看工作区和版本库里面最新版本的区别
* git checkout -- readme.txt 撤销修改 把readme.txt文件在工作区的修改全部撤销，

  一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
  
  一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
  总之，让这个文件回到最近一次git commit或git add时的状态。

### git特点
* 管理的是修改而不是文件

