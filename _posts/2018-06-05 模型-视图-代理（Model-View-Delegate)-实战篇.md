


```
#ifndef SPINBOXDELEGATE_H
#define SPINBOXDELEGATE_H
#include <QItemDelegate>
#include <QObject>
#include <QSpinBox>

class SpinBoxDelegate : public QItemDelegate
{

    Q_OBJECT

public:

    SpinBoxDelegate(QObject *parent = 0);

    QWidget *createEditor(QWidget *parent, const QStyleOptionViewItem &option, const QModelIndex &index) const;

    void setEditorData(QWidget *editor, const QModelIndex &index) const;













```
#include "spinboxdelegate.h"
#include <QSpinBox>
#include <QItemDelegate>

SpinBoxDelegate::SpinBoxDelegate(QObject *parent)
    : QItemDelegate(parent)
{
}

QWidget *SpinBoxDelegate::createEditor(QWidget *parent, const QStyleOptionViewItem &/* option */,
    const QModelIndex &/* index */) const
{
    QSpinBox *editor = new QSpinBox(parent);

    editor->setRange(0,10000);

    editor->installEventFilter(const_cast<SpinBoxDelegate*>(this));

    return editor;

}

void SpinBoxDelegate::setEditorData(QWidget *editor, const QModelIndex &index) const
{
    int value = index.model()->data(index, Qt::EditRole).toInt();

    QSpinBox *spinBox = static_cast<QSpinBox*>(editor);

    spinBox->setValue(value);

}

void SpinBoxDelegate::setModelData(QWidget *editor, QAbstractItemModel *model,
                                   const QModelIndex &index) const
{
    QSpinBox *box = static_cast<QSpinBox*>(editor);
    int value = box->value();
    model->setData(index,value);
}

void SpinBoxDelegate::updateEditorGeometry(QWidget *editor,
    const QStyleOptionViewItem &option, const QModelIndex &/* index */) const
{
    editor->setGeometry(option.rect); // option.rect属性中保存了条目的位置，这里将控件设置在刚好占住条目的位置
}

```

必须要重写前三个函数：
- 1.`QWidget*createEditor(）`       提供一个编辑器,告诉model用的是那个编辑器进行数据的编辑和选择
- 2.`void setEditorData(）  `      为editor提供编辑的原始数据, 从model中读取数据，设置编辑器显示的数据
- 3.`void setModelData( ）  `       根据editor 的数据更新model的数据,从编辑器中读取当前显示的数据，设置model中的数据.
- 4.`void updateEditorGeometry( ）  `       保证editor显示在 item view 的合适位置以及大小

**MyWidget** 构造函数

```



QStandardItemModel>
#include<QTableView>
#include<QHBoxLayout>

MyWidget::MyWidget(QWidget *parent) :
    QWidget(parent),
    ui(new Ui::MyWidget)
{
    ui->setupUi(this);

    //定义一个模型 大小是4*4 的
    QStandardItemModel *tableModel=new QStandardItemModel(4,4,this);

    //定义一个SpinBox功能的代理
    SpinBoxDelegate *spinDelegate=new SpinBoxDelegate;

    //设置表头名称
    tableModel-> setHeaderData（0，Qt的::水平，TR（ “帮派”））; 
    tableModel-> setHeaderData（1，Qt的::水平， “姓名”）; 
    tableModel-> setHeaderData（2，Qt的::水平“花名”）; 
    tableModel-> setHeaderData（3，Qt的::水平， “排行”）;     //显示    QTableView * tabletView = new QTableView;     //显示的模式    tabletView->则setModel（TableModel的）;     //代理作用于第三列    tabletView-> setItemDelegateForColumn（3，spinDelegate）;     //设置显示的布局和大小    QHBoxLayout * mainLayout = new QHBoxLayout（this）;     mainLayout-> addWidget（tabletView）;     这- >调整（600,400）; } 进myWidget ::〜进myWidget（）{     删除UI;





















[这里写图片描述](https://img-blog.csdn.net/20180729163905142?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09zZWFuX2xp/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

#小结

在上面重写的四个方法中
首先执行的是
- 1.`QWidget*createEditor(）`                 设置代理成功了，创建编辑区
- 2.`void updateEditorGeometry( ）  ` 随后设置显示Item的位置
- 3.`void setEditorData(）  `                接着给编辑区设置数据 是给Spinbox 赋值
- 4.`void setModelData( ）  `                数据在同步到模型上是给模型设置值

**猜测**在UI上的显示在第三部就已经显示了但是为了MV的数据同步，所以在给模型设置数据，来保证步骤3,4的数据的同步与一致性。若问题，请指正！














