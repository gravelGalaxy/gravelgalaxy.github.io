---
title: graphics
date: 2024-01-15 10:22:04
tags: Qt
---

# Graphics View 架构的组成
场景、视图、图形项
## 场景
QGraphicsScene类：管理图形项的容器。

1. 提供管理大量图形项的快速接口。
2. 将事件传播给每个图形项。
3. 管理每个图形项的状态，（选择状态，焦点状态等）
4. 管理未经变换的渲染功能（打印）。

场景也分背景层和前景层，可以用来实现自定义的背景和前景。
## 视图
QGraphicsView类：显示场景中的内容。

- 场景视图：可以为一个场景设置几个不同的视图，对同一个数据集提供不同的视口。
- 场景事件：视图接收键盘和鼠标输入并转换为场景事件，并进行坐标转换后传送给可视场景。

### 坐标系统
包括图形项坐标、场景坐标、视图坐标。
#### 图形项坐标 Item Coordinates
局部逻辑坐标，一般以图件的`中心`为原点。
- 自定义图形项：绘制图形项时只需要考虑局部坐标。
- 图形项的坐标：中心点在父坐标系统中的坐标。

#### 场景坐标
等价于QPainter的逻辑坐标。一般以场景的`中心`为原点。

- 所有图形项的基础坐标。
```
scene = new QGraphicsScene(-400, -300, 800, 600);
```
- 边界矩阵：可以使QGraphicsScene知道场景的哪个区域发生了变化。
> 场景发生变化时会发射QGraphicsScene::changed()信号，参数是一个场景的矩形列表，表示发生变化的矩形区。

#### 视图坐标
等价于设备坐标（物理坐标），缺省是以`左上角`为原点。
- 视图坐标只与widget或viewport有关，而与观察的场景无关。viewport左上角的坐标总是（0, 0）。
- 所有的鼠标事件、拖放事件的坐标首先是由视图坐标定义的。之后需要映射为场景坐标，以便于图形项交互。

#### 坐标映射 Coordinate Mapping
场景 --> 图形项
图形项 --> 图形项
视图 --> 场景

```
QGraphicsView::mapToScene();    // 将视图坐标映射为场景坐标
QGraphicsScene::itemAt();       // 获取场景中鼠标光标处的图形项
```

## 图形项
QGraphicsItem类：图形项的基类。

1. 支持鼠标事件相应。
2. 支持键盘输入、按键事件。
3. 支持拖放操作。
4. 支持组合，可以是父子项关系的组合，也可以是通过QGraphicsItemGroup类进行组合。
5. 支持碰撞检测。

```
setZValue();    //控制图形项的叠放次序，当有多个图形项重叠时，zValue越大的，越显示在前面。
```

# 实例
```
void Widget::initGraphicsSystem()
{
    // 创建一个可选中的，可以作为焦点的矩形
    QRectF rect(-200, -100, 400, 200);  //以中心为(0, 0)
    scene = new QGraphicsScene(rect);
    view->setScene(scene);
    QGraphicsRectItem *item = new QGraphicsRectItem(rect);
    item->setFlags(QGraphicsItem::ItemIsSelectable | QGraphicsItem::ItemIsFocusable);

    QPen pen;
    pen.setWidth(2);
    item->setPen(pen);
    scene->addItem(item);

    // 创建一个可移动、可选中、可以作为焦点的椭圆
    QGraphicsEllipseItem *item2 = new QGraphicsEllipseItem(-100, -50, 200, 100);
    item2->setPos(0, 0);
    item2->setBrush(QBrush(Qt::blue));
    item2->setFlags(QGraphicsItem::ItemIsMovable
                    | QGraphicsItem::ItemIsSelectable
                    | QGraphicsItem::ItemIsFocusable
                    );
    scene->addItem(item2);
    scene->clearSelection();
}
```
