---
title: 04 scikit-learn简介
toc: true
date: 2018-07-08 13:33:35
---

# 13.4 Introduction to scikit-learn（scikit-learn简介）


scikit-learn是一个被广泛使用的 Python 机器学习工具包。里面包含了很多监督式学习和非监督式学习的模型，可以实现分类，聚类，预测等任务。

虽然 scikit-learn并没有和 pandas 深度整合，但在训练模型之前，pandas在数据清洗阶段能起很大作用。

> 译者：构建的机器学习模型的一个常见流程是，用 pandas 对数据进行查看和清洗，然后把处理过的数据喂给 scikit-learn中的模型进行训练。

这里用一个经典的 kaggle 比赛数据集来做例子，泰坦尼克生还者数据集。加载训练集和测试集：


```Python
import numpy as np
import pandas as pd
```


```Python
train = pd.read_csv('../datasets/titanic/train.csv')
test = pd.read_csv('../datasets/titanic/test.csv')
```


```Python
train.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>Braund, Mr. Owen Harris</td>
      <td>male</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>A/5 21171</td>
      <td>7.2500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>Cumings, Mrs. John Bradley (Florence Briggs Th...</td>
      <td>female</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>PC 17599</td>
      <td>71.2833</td>
      <td>C85</td>
      <td>C</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>1</td>
      <td>3</td>
      <td>Heikkinen, Miss. Laina</td>
      <td>female</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>STON/O2. 3101282</td>
      <td>7.9250</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>Futrelle, Mrs. Jacques Heath (Lily May Peel)</td>
      <td>female</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>113803</td>
      <td>53.1000</td>
      <td>C123</td>
      <td>S</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>0</td>
      <td>3</td>
      <td>Allen, Mr. William Henry</td>
      <td>male</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>373450</td>
      <td>8.0500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>



statsmodels和 scikit-learn通常不能应付缺失值，所以我们先检查一下哪些列有缺失值：


```Python
train.isnull().sum()
```




    PassengerId      0
    Survived         0
    Pclass           0
    Name             0
    Sex              0
    Age            177
    SibSp            0
    Parch            0
    Ticket           0
    Fare             0
    Cabin          687
    Embarked         2
    dtype: int64




```Python
test.isnull().sum()
```




    PassengerId      0
    Pclass           0
    Name             0
    Sex              0
    Age             86
    SibSp            0
    Parch            0
    Ticket           0
    Fare             1
    Cabin          327
    Embarked         0
    dtype: int64



对于这样的数据集，通常的任务是预测一个乘客最后是否生还。在训练集上训练模型，在测试集上验证效果。

上面的 Age 这一列有缺失值，这里我们简单的用中位数来代替缺失值：


```Python
impute_value = train['Age'].median()
train['Age'] = train['Age'].fillna(impute_value)
test['Age'] = test['Age'].fillna(impute_value)
```

对于 Sex 列，我们将其变为 IsFemale，用整数来表示性别：


```Python
train['IsFemale'] = (train['Sex'] == 'female').astype(int)
test['IsFemale'] = (test['Sex'] == 'female').astype(int)
```


```Python
train.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
      <th>IsFemale</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>Braund, Mr. Owen Harris</td>
      <td>male</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>A/5 21171</td>
      <td>7.2500</td>
      <td>NaN</td>
      <td>S</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>Cumings, Mrs. John Bradley (Florence Briggs Th...</td>
      <td>female</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>PC 17599</td>
      <td>71.2833</td>
      <td>C85</td>
      <td>C</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>1</td>
      <td>3</td>
      <td>Heikkinen, Miss. Laina</td>
      <td>female</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>STON/O2. 3101282</td>
      <td>7.9250</td>
      <td>NaN</td>
      <td>S</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>Futrelle, Mrs. Jacques Heath (Lily May Peel)</td>
      <td>female</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>113803</td>
      <td>53.1000</td>
      <td>C123</td>
      <td>S</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>0</td>
      <td>3</td>
      <td>Allen, Mr. William Henry</td>
      <td>male</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>373450</td>
      <td>8.0500</td>
      <td>NaN</td>
      <td>S</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



接下来决定一些模型参数并创建 numpy 数组：


```Python
predictors = ['Pclass', 'IsFemale', 'Age']
```


```Python
X_train = train[predictors].values
X_test = test[predictors].values
y_train = train['Survived'].values
```


```Python
X_train[:5]
```




    array([[  3.,   0.,  22.],
           [  1.,   1.,  38.],
           [  3.,   1.,  26.],
           [  1.,   1.,  35.],
           [  3.,   0.,  35.]])




```Python
y_train[:5]
```




    array([0, 1, 1, 1, 0])



这里我们用逻辑回归模型（LogisticRegression）：


```Python
from sklearn.linear_model import LogisticRegression
```


```Python
model = LogisticRegression()
```

然后是 fit 方法来拟合模型：


```Python
model.fit(X_train, y_train)
```




    LogisticRegression(C=1.0, class_weight=None, dual=False, fit_intercept=True,
              intercept_scaling=1, max_iter=100, multi_class='ovr', n_jobs=1,
              penalty='l2', random_state=None, solver='liblinear', tol=0.0001,
              verbose=0, warm_start=False)



在测试集上进行预测，使用 model.predict:


```Python
y_predict = model.predict(X_test)
y_predict[:10]
```




    array([0, 0, 0, 0, 1, 0, 1, 0, 1, 0])



如果我们有测试集的真是结果的话，可以用来计算准确率或其他一些指标：

    (y_true == y_predcit).mean()
    
实际过程中，训练模型的时候，经常用到交叉验证（cross-validation），用于调参，防止过拟合。这样得到的预测效果会更好，健壮性更强。

交叉验证是把训练集分为几份，每一份上又取出一部分作为测试样本，这些被取出来的测试样本不被用于训练，但我们可以在这些测试样本上验证当前模型的准确率或均方误差（mean squared error），而且还可以在模型参数上进行网格搜索（grid search）。一些模型，比如逻辑回归，自带一个有交叉验证的类。LogisticRegressionCV类可以用于模型调参，使用的时候需要指定正则化项 C，来控制网格搜索的程度：


```Python
from sklearn.linear_model import LogisticRegressionCV
```


```Python
model_cv = LogisticRegressionCV(10)
```


```Python
model_cv.fit(X_train, y_train)
```




    LogisticRegressionCV(Cs=10, class_weight=None, cv=None, dual=False,
               fit_intercept=True, intercept_scaling=1.0, max_iter=100,
               multi_class='ovr', n_jobs=1, penalty='l2', random_state=None,
               refit=True, scoring=None, solver='lbfgs', tol=0.0001, verbose=0)



如果想要自己来做交叉验证的话，可以使用 cross_val_score函数，可以用于数据切分。比如，把整个训练集分为 4 个不重叠的部分：


```Python
from sklearn.model_selection import cross_val_score
```


```Python
model = LogisticRegression(C=10)
model
```




    LogisticRegression(C=10, class_weight=None, dual=False, fit_intercept=True,
              intercept_scaling=1, max_iter=100, multi_class='ovr', n_jobs=1,
              penalty='l2', random_state=None, solver='liblinear', tol=0.0001,
              verbose=0, warm_start=False)




```Python
scores = cross_val_score(model, X_train, y_train, cv=4)
scores
```




    array([ 0.77232143,  0.80269058,  0.77027027,  0.78828829])



默认的评价指标每个模型是不一样的，但是可以自己指定评价函数。交差验证的训练时间较长，但通常能得到更好的模型效果。
