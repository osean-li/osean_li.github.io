引言在我们进行Qt开发的时候，Qt为我们提供了QListWidget，QTableWidget，QTreeWidget，他们的基类分别是QListView，QTableView，QTreeView。这几个类特点是使用起来很方便，适合显示比较简单的数据，若是涉及到大量的数据要显示，以及对性能要求严格就得用到视图模型了。二者的区别在于QListWidget = QListView + Model 







＃模型我们先来看看模型！[ 这里写图片描述] （https://img-blog.csdn.net/20180729151927435?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09zZWFuX2xp/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70 ）可以把模型嵌套在View上。每个模型中中都有Item，来操控每一项。下面使用Tree Model作为例子！[ 这里写图片描述] （






https://img-blog.csdn.net/2018072915280122?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09zZWFuX2xp/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

- Index用于索引model中的数据，相当于指针下标之类的效果

上图中，A项和C项作为model中顶层的兄弟项：

```
QModelIndex indexA = model->index(0, 0, QModelIndex());
QModelIndex indexC = model->index(2, 1, QModelIndex());
```

A有许多孩子，它的一个孩子B用以下代码获取：

```
QModelIndex indexB = model->index(1, 0, indexA);
```

通过上面几行代码，可以知道最后一个参数的作用。

#视图
视图就是让数据显示出来，然而却无法和用户进行交互，此时便需要代理。

#代理

delegate更灵活的处理用户的输入，能够自定义数据条目（item）的显示和编辑方式。

#三者的关系

它们之间的关系如下：
     - 数据发生改变时，模型发出信号通知视图。     -用户对界面进行操作，视图发生信号。     -代理发出信号告知模型和视图编辑器目前的状态。



关系图如下！[ 这里写图片描述] （https://img-blog.csdn.net/2018072915380652?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09zZWFuX2xp/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70 ）！[ 这里写图片描述] （https://img-blog.csdn.net/20180729154006322?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09zZWFuX2xp/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70 ）






