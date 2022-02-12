---
title: Decision_Tree
date: 2021-08-26 22:02:54
tags: [ML, 数据分析]
categories: ML
---

# 决策树概述
+ 算法核心： 

  + 如何从数据表中找出最佳节点和最佳分枝？
  + 如何让决策树停止生长，防止过拟合？

  <!--more-->

+ sklearn中的决策树 （模块sklearn.tree）

  | tree.DecisionTreeClassifier | 分类树                                |
  | --------------------------- | ------------------------------------- |
  | tree.DecisionTreeRegressor  | 回归树                                |
  | tree.export_graphviz        | 将生成的决策树导出为DOT格式，画图专用 |
  | tree.ExtraTreeClassifier    | 高随机版本的分类树                    |
  | tree.ExtraTreeRegressor     | 高随机版本的回归树                    |

+ sklearn的基本建模流程

  + 导入模块
  + 实例化 - 建立模型 
  + 训练模型 model = model.fit(X_train, Y_train)
  + 通过模型接口获得信息 (score, apply, predict...)

# DecisionTreeClassifier与红酒数据集

+ 决策树的基本流程
  + 计算全部特征的不纯度
  + 选取最优不纯度来分支
  + 计算条件不纯度，选取最优不纯度来分支。重复直到整体不纯度最优或无多余特征

class `sklearn.tree.DecisionTreeClassifier` 

(**criterion**=’gini’, **splitter**=’best’, **max_depth**=None, **min_samples_split**=2, **min_samples_leaf**=1, **min_weight_fraction_leaf**=0.0, **max_features**=None, **random_state**=None, max_leaf_nodes=None, **min_impurity_decrease**=0.0, **min_impurity_split**=None, **class_weight**=None, **presort**=False)

+ 确认最优的剪枝参数 --> 超参数的曲线
+ 八个参数：
  + Criterion
  + 两个随机性相关的参数（random_state，splitter）
  + 五个剪枝参数（max_depth, min_samples_split，min_samples_leaf，max_feature，min_impurity_decrease）
+ 一个属性：feature_importances_
+ 四个接口：fit，score，apply，predict

## 重要参数

###  criterion （“entropy” “gini”）

+ 作用：决定不纯度的计算方法 

  + 不纯度越低，决策树对训练集的拟合越好
  + 子节点的不纯度一定是低于父节点

+ 选择：（“entropy” “gini”）（default - gini）

  + 信息熵（Entropy）: 实际计算的是基于信息熵的信息增益(Information Gain) --> 父子差
  + 基尼系数（Gini Impurity）

  区别：欠拟合使用信息熵，过拟合使用基尼系数

  + 信息熵对不纯度更加敏感，对不纯度的惩罚最强。决策树的生长会更加“精细”。对于高维数据或者噪音很多的数据，信息熵很容易过拟合。
  + 信息熵的计算比基尼系数缓慢一些，因为基尼系数的计算不涉及对数。

### random_state

+ 作用：设置分枝中的随机模式的参数。默认None

### splitter("random" "best")

+ 作用：控制决策树中的随机选项

+ 选择：("random" "best")

  + best: 优先选择更重要的特征进行分枝 feature_importances_
  + random: 更随机，树会因为含有更多的不必要信息而更深更大，并因这些不必要信息而降低对训练集的拟合 --> 防止过拟合 (树一旦建成，我们依然是使用剪枝参数来防止过拟合)

### max_depth

+ 限制树的最大深度，超过设定深度的树枝全部剪掉
+ 在高维度低样本量时非常有效。决策树多生长一层，对样本量的需求会增加一倍，所 以限制树深度能够有效地限制过拟合
+ 建议从=3开始尝试，看看拟合的效 果再决定是否增加设定深度

### min_samples_leaf

+ 一个节点在分枝后的每个**子节点**都必须包含至少min_samples_leaf个训练样本，否则分 枝就不会发生，或者，分枝会朝着满足每个子节点都包含min_samples_leaf个样本的方向去发生
+ 这个参数的数量设置得太小会引 起过拟合，设置得太大就会阻止模型学习数据
+ 建议从=5开始使用。如果叶节点中含有的样本量变化很大，建议输入浮点数作为样本量的百分比来使用

### min_samples_split

+ 一个**节点**必须要包含至少min_samples_split个训练样本，这个节点才允许被分枝。即中间节点包含样本数必须大于min_samples_split

### max_features

+ 限制分枝时考虑的特征个数，超过限制个数的特征都会被舍弃
+ 过于暴力，一般不推荐是使用（优先考虑PCA, ICA...）

### min_impurity_decrease

+ 限制信息增益的大小，信息增益小于设定数值的分枝不会发生。其目的在于删去贡献小的分支。

### class_weight & min_weight_fraction_leaf

+ 样本不平衡的时候使用

## 代码

```python
from sklearn import tree
from sklearn.datasets import load_wine
from sklearn.model_selection import train_test_split
```

### 探索数据


```python
import pandas as pd
wine = load_wine()
wine.data.shape
wine.target

pd.concat([pd.DataFrame(wine.data), pd.DataFrame(wine.target)],
          axis=1)  # axis = 0 (row) / 1(column)

wine.feature_names
wine.target_names
```




    array(['class_0', 'class_1', 'class_2'], dtype='<U7')



### 训练集和测试集 train_test_split


```python
Xtrain, Xtest, Ytrain, Ytest = train_test_split(
    wine.data, wine.target, test_size=0.3)  # 7:3
Xtrain.shape
Xtest.shape
```




```python
(54, 13)
```



### 建立模型 剪枝/确定超参数


```python
model = tree.DecisionTreeClassifier(criterion = "entropy", random_state = 30, splitter = "best", max_depth = 3
                                   # , min_samples_leaf=5, min_samples_split= 5
                                   )
model = model.fit(Xtrain, Ytrain)
score = model.score(Xtest,Ytest)

print(model.score(Xtrain,Ytrain))
score
```

    0.9838709677419355





    0.9074074074074074




```python
import matplotlib.pyplot as plt

test = []

for i in range(10):  # 0-9
    model = tree.DecisionTreeClassifier(max_depth=i+1, criterion="entropy", random_state=30, splitter="best"
                                        )
    model = model.fit(Xtrain, Ytrain)
    score = model.score(Xtest, Ytest)
    test.append(score)

plt.plot(range(1, 11), test, color="red",
         label="max_depth")  # range(1,11):1-10
plt.legend()
plt.show()
```


​    ![output_7_0](C:\Users\Ashley Li\Desktop\Ashley37sky.github.io\source\_posts\image\output_7_0.png)


### 画图


```python
feature_names = wine.feature_names
import graphviz

dot_data = tree.export_graphviz(model  # 逗号写在前面方便注释
                               , out_file = None
                               , feature_names = feature_names
                               , class_names= ["可乐","雪碧","橙汁"]
                               , filled = True # 填充颜色（颜色：类别，深浅：不纯度）
                                , rounded = True # 椭圆边框
                               )
graph = graphviz.Source(dot_data)
graph
```




​    

![output_9_0](C:\Users\Ashley Li\Desktop\Ashley37sky.github.io\source\_posts\image\output_9_0.svg)

### 探索决策树


```python
model.feature_importances_
[*zip(feature_names, model.feature_importances_)]
```




    [('alcohol', 0.0),
     ('malic_acid', 0.0),
     ('ash', 0.0),
     ('alcalinity_of_ash', 0.0),
     ('magnesium', 0.059412770833065044),
     ('total_phenols', 0.11768999894990434),
     ('flavanoids', 0.03239612249873199),
     ('nonflavanoid_phenols', 0.0),
     ('proanthocyanins', 0.03491820348040222),
     ('color_intensity', 0.01848749117382521),
     ('hue', 0.05517932728388334),
     ('od280/od315_of_diluted_wines', 0.2425246325705508),
     ('proline', 0.439391453209637)]



### 测试样本


```python
model.apply(Xtest) # 返回测试样本所在的叶子节点
model.predict(Xtest)
```




    array([1, 0, 0, 1, 1, 1, 1, 0, 1, 1, 2, 1, 0, 2, 1, 2, 0, 0, 2, 0, 0, 1,
           1, 2, 1, 0, 0, 1, 0, 1, 1, 2, 2, 0, 1, 2, 2, 2, 0, 1, 1, 2, 1, 2,
           0, 0, 1, 0, 1, 1, 1, 1, 0, 2])

