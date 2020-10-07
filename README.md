# 多边形裁剪实验报告

## 实验目的
实现多边形裁剪算法并通过图形界面接收用户输入的裁剪多边形和主多边形，完成多边形的裁剪，显示裁剪结果
</br></br>

## 实验方法
- **多边形顶点序列方向**  
  
  - 对于一般凸多边形而言，只需对其某一顶点 `i` 计算叉积，即：

    $((x_i - x_{i-1}), (y_i - y_{i-1})) * ((x_{i+1} - x_i), (y_{i+1} - y_i))$  

    $= (x_i - x_{i-1}) * (y_{i+1} - y_i) - (y_i - y_{i-1}) * (x_{i+1} - x_i)$  
    若上式为正，则点序列为逆时针，为负则为顺时针

  - 对于一般简单多边，则需对于多边形顶点序列中的每一个点计算上述叉积，若正值数量多于负值则为逆时针，负值数量多于正值数量则为顺时针
  - 对于环，首先计算最外环方向，对内环，逐级向内顺逆时针交替以定义环中顶点序列方向。如，最外环为顺时针，则其内环为逆时针，再次一级内环为顺时针
  - 实验中首先计算裁剪多边形与主多边形方向，若有环则计算最外环方向，若方向一致则直接运行多边形裁剪算法，否则将主多边形顶点序列转置后再进行多边形裁剪

- **顶点与多边形位置关系**  
  
  顶点于多边形位置关系包含以下几种：
  - 点在多边形內部
  - 点在多边形边上
  - 点在多边形外部
  
  实验中使用射线法进行判断，从判断点向某个统一方向作射线，依交点个数的奇偶判断，若为奇，点在多边形内部或边上，若为偶，则点在多边形外。具体实现时，使用循环逐点计算叉积，并对正负值计数，比较数量以判断位置关系
  
- **多边形裁剪**  

  使用 `Weiler-Atherton` 算法实现。 `WA` 算法中裁剪多边形和主多边形对顶点序列按顺时针方向排列。当两个多边形存在相交时，交点必然成对出现，一个是由主多边形边进入裁剪多边形的交点，称为“入点”，另一个是由主多边形边离开裁剪多边形的交点，称为“出点”，出点、入点成对存在。由主多边形的一个入点开始，沿主多边形边及裁剪多边形边按顺时针方向得到顶点及交点序列序列。从该入点出发，沿主多边形点序列逐个添加入结果列表。遇到出点，则沿裁剪多边形点序列加入结果列表，直到遇到下一入点或遍历所有点。此时得到的结果列表按序连接即为裁剪结果。  
</br></br>

## 实验结果
裁剪结果如下：
<div style="display: flex;">
  <div style="width: 100%;">
    <img src="https://tva1.sinaimg.cn/large/007S8ZIlly1gjgu5x0qgrj311o0u04be.jpg">
    <div style="text-align: center;">图1.1 裁剪多边形及主多边形</div>
  </div>
  <div style="width: 100%;">
    <img src="https://tva1.sinaimg.cn/large/007S8ZIlly1gjgu5cyrmdj311o0u0tll.jpg">
    <div style="text-align: center;">图1.2 裁剪结果</div>
  </div>
</div>  
</br></br>

## 问题分析

- **出入点获取**  

  对主多边形与裁剪多边形遍历两者的边序列，两两组合计算交点，获得所有的交点的集合即出入点的集合

- **出入点判断**  

  使用上述顶点与多边形位置关系计算出首个出/入点前一点与裁剪多边形的位置关系，若前一点在裁剪多边形內/边上，则该点为出点，否则为入点。此后的交点按出点、入点的顺序交替出现。遍历交点集即可判断所有出入点
</br></br>

## 交互方式
- **多边形输入**  
  
  在多边形窗口点击左键以输入多边形顶点，点击右键以封闭多边形。边与边的相交是被默认禁止的，所以点击添加点以添加边的操作会导致边交叉时，此次点击无效（不添加边/点）
- **多边形切换**  
  
  点击选择左下角 `radioButton` 以切换裁剪多边形和主多边形的输入
- **裁剪**  
  
  点击右下角裁剪按钮以裁剪多边形
- **清空**  
  
  点击右下角清空按钮以清空画布，重新输入裁剪多边形和主多边形
</br></br>

## 编译环境
- `Python 3.8`
</br></br>

## 编译方式
``` zsh
pip install -r requirements.txt
python polygon.py
```

