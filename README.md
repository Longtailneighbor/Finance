

[DataCastle 的用户贷款风险预测比赛链接](http://www.pkbigdata.com/common/cmpt/%E7%94%A8%E6%88%B7%E8%B4%B7%E6%AC%BE%E9%A3%8E%E9%99%A9%E9%A2%84%E6%B5%8B_%E5%8F%82%E8%B5%9B%E4%B8%8E%E7%BB%84%E9%98%9F.html#teamStandard)

**第一次参数能排进前 10/2000+，很满意了**

> 线上排名是当时的比赛排名


![最后排名][1]



特征工程

-  user info 的性别特征，利用了决策树的特性直接使用，没有进行独热编码，缺失性别赋值 -9999，交给模型处理，模型对缺失值的处理是：把缺失值放在能最小化目标函数的子分支，这样处理的 sex 特征评分明显增大


-  bill 数据的特征，有用到分放款前后，以及对比；按照数值分为了 10 段，对用户的每个数据段进行了统计，虽然产生了不少空值列，但是同时也产生了一批评分很高的特征，也就是说，有些用户能够通过数额较大的消费能够反映出来；也统计了一些专业方面的特征比如 FICO 方面的数据，新增的信用卡数量，信用卡的使用年限，最近使用的信用卡，最近的信用情况，历史信用违约数据，但是这些数据没有想象中起作用，最后考虑到 bill 数据的规模还是放弃掉了

- bank 数据特征，银行数据并没有什么好用的特征，包括统计了银行卡的张数，收入支出情况，以及收入支出的差值情况，分时段统计，分数值统计都没有什么好用的特征


- browser 数据特征，这里的类别信息比较多，也就是简单的进行类别统计，但是特征评分挺高的


模型

- 前期使用了 XGBoost，GBDT，但是 XGBoost 确实挺快的，而且可调参的地方更加全面和实用，所以最后 XGBoost 杀到底，不可思议的是，在距离比赛结束还有两天的时候，XGBoost 单个模型能跑到了排行榜第 8 名

模型融合
-  做模型融合的时候，距离比赛结束还剩 3 天，没有深入的研究，做了简单的加权融合，提升了不少，另外还做了 rank 融合，感觉 rank 融合把概率值直接看做排名，抛弃掉了原有的概率信息，原来的概率是通过最小化目标函数算出来的，而 rank 之后这些信息就没有了，有点不解，不过倒是能够处理掉不同模型之间的差异，尝试的不够多，效果没有显现出来 


stacking

-  模型之上再加模型，感觉上挺好，但是代价就是拆分了特征，单个模型的效果会下降，上层的模型缺乏验证数据集，而 CV 又容易过拟合，最后的效果比不上加权融合


  [1]: ./images/1487765911620.jpg "1487765911620.jpg"
  
