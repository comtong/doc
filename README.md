Com的财富
=======
android 学习笔记
-----------
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

