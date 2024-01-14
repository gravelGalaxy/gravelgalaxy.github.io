---
title: QPainter
date: 2024-01-14 09:27:03
tags: Qt
---

# 绘图设备
常见的有QWidget, QPixmap, QImage等(QWidget类及其子类是最常用的绘图设备)，作用是给QPainter提供一个“画布”。
# 坐标系统
## 物理坐标
## 逻辑坐标
viewport坐标: 物理坐标的一个矩形区域。默认是整个Widget。
window坐标: 当实际绘图设备发生变化时，绘制的图形会自动变化大小。将viewport坐标用凭空给定的坐标`替代`, 不必知道viewport的大小。
# 通过QPainter在绘图设备上画图
QWidget类及其子类有paintEvent()事件，重写此事件的相应代码即可在设备上绘图。其基本结构如下所示：
```
void Widget::paintEvent(QPainterEvent *event)
{
	QPainter painter(this);// 创建与绘图设备相关联的QPainter
	//painter 在设备的窗口上绘图
}
```
绘图时，会用到QPainter的3个属性：
1. pen:QPen对象，设置线条的颜色、宽度、线型。
2. brush:QBrush对象，设置区域的填充颜色、填充方式、渐变等。
3. font:QFont对象，设置文字的字体、大小等。

可以在Qt中新建一个QWidget项目，并在头文件中声明对paintEvent的重写，在源文件中实现：
```

void Widget::paintEvent(QPaintEvent *event)
{
    QPainter painter(this);
    QPen pen;
    QBrush brush;

    painter.setRenderHint(QPainter::Antialiasing);
    painter.setRenderHint(QPainter::TextAntialiasing);

    // 要绘制的形状。通过QPainter进行绘制，但在绘制前应设置QPainter的pen和brush属性。
    int w = this->width();
    int h = this->height();
    QRect rect(w / 4, h / 4, w / 2, h / 2);

    // 设置pen
    pen.setWidth(3);
    pen.setColor(Qt::green);
    pen.setStyle(Qt::SolidLine);	// 设置线条样式
    pen.setCapStyle(Qt::FlatCap);	// 设置线条端点样式
    pen.setJoinStyle(Qt::BevelJoin);	// 设置连接样式
    painter.setPen(pen);

    // 设置brush
    brush.setColor(Qt::gray);
    brush.setStyle(Qt::SolidPattern);
    painter.setBrush(brush);

    // 绘图
    painter.drawRect(rect);
}
```
# 绘制基本图形元件
在绘制图形元件时，不可避免的要进行数字计算。例如当我们要绘制一个五角星时，该怎样得到五角星每个顶点的坐标呢？
1. 首先我们将五角星放到圆内，那么圆心就是五角星的中心。我们假设五角星的某个点与圆心之间的连线是x轴，逆时针旋转90度的为y轴。因为圆心与五角星的五个顶点之间的连线将圆分成了5份，那么每一份的角度是72度。因此，其他的点的坐标就可以通过sin和cos计算出来。
```
void Widget::paintEvent(QPaintEvent *event) 
{
	QPainter painter(this);
	
	qreal r = 100;
	const qreal pi = 3.14159;
	qreal deg = pi * 72 / 180;
	QPoint points[5] = {
		QPoint(r, 0);
		QPoint(r * std::cos(deg), r * std::sin(deg));
		QPoint(r * std::cos(2 * deg), r * std::sin(2 * deg));
		QPoint(r * std::cos(3 * deg), r * std::sin(3 * deg));
		QPoint(r * std::cos(4 * deg), r * std::sin(4 * deg));
	};
	
	QPen pen;
	pen.setColor(Qt::blue);
	pen.setStyle(Qt::SolidLine);
	pen.setCapStyle(Qt::FlatCap);
	pen.setJoinStyle(Qt::BevelJoin);
	painter.setPen(pen);
	
	QPainterPath starPath;
	starPath.moveTo(points[0]);
	starPath.moveTo(points[2]);
	starPath.moveTo(points[4]);
	starPath.moveTo(points[1]);
	starPath.moveTo(points[3]);
	starPath.closeSubpath();	//闭合路径

	painter.save();
	painter.translate(100, 120);
	painter.drawPath(starPath);
	painter.restore();
	
	painter.translate(300, 120);
	painter.scale(0.8, 0.8);
	painter.rotate(90);
	painter.drawPath(starPath);
	painter.restore();
	
	painter.resetTransform();
	painter.translate(500, 120);
	painter.rotate(-145);
	painter.drawPath(starPath);
}
	
```
	


# 坐标变换
逻辑坐标系统：你可以决定绘图设备上的某个点作为原点，以此原点建立的坐标系是逻辑坐标。
1. translate(qreal dx, qreal dy):将原点通过dx和dy`偏移量`移动到新的点。
2. rotate(qreal angle):坐标系统顺时针旋转angle角度。
3. scale(qreal sx, qreal sy):坐标系统缩放。
4. resetTransform():复位所有的坐标变换。
5. save():将painter当前状态压入堆栈。
6. restore():从堆栈中恢复上一次的状态。

# Questions
### 为什么QGradient子类可以作为QBrush传递给QPainter？
因为QBrush中包含构造函数：
```
QBrush(const QGradient &gradient);
```
所以，当给painter.setBrush(gradient)时，会自动将gradient构造成QBrush对象。
### 为什么QPainter,QBrush,QPen这些对象保存到栈中，而非堆中？
因为Qt通过使用画布上的像素点来表现图形，将图像保存到绘图设备上，直到该绘图设备被销毁或者图形被重新绘制。


# 样式
## 线条样式
1. Qt::SolidLine
2. Qt::DashLine
3. Qt::DotLine
4. Qt::DashDotLine
5. Qt::DashDotDotLine
6. Qt::CustomDashLine
## 线条端点样式
1. Qt::SquareCap	// 线条端点是矩形棱角，端点是方正的
2. Qt::FlatCap		// 线条断点也是棱角，但是是倾斜的。
3. Qt::RoundCap		// 线条断点是圆角
## 线条连接样式
1. Qt::BevelJoin	// 线条连接部分有棱角，但是不是尖锐的。
2. Qt::MiterJoin	// 线条连接部分有棱角，并且是尖锐的。
3. Qt::RoundJoin	// 线条连接部分是圆角。

## 画刷样式
1. Qt::NoBrush			// 无填充
2. Qt::SolidPattern		// 单一颜色填充
3. Qt::HorPattern		// 水平线填充
4. Qt::VerPattern		// 垂直线填充
5. Qt::LinearGradientPattern	// 线性渐变
6. Qt::RadialGradientPattern	// 辐射渐变
7. Qt::ConicalGradientPattern	// 圆锥形渐变
8. Qt::TexturePattern		// 材质填充


