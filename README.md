alpha, 多因子选股
================

从数据库提取数据构建因子，测试因子的有效性，包括因子收益率、因子收益率T值、IC值，分层测试以观察因子收益率的按照因子大小排序分组的不同组合的组合收益率、波动率、收益率的单调性、最大回测、夏普比率、信息比率等。

## factor_code, 因子的构建和测试  

- 估值类因子（7个）
- 动量类因子（6个）
- 波动率类因子（10个）
- 一致预期类因（18个）

## self_libs, 自定义的一些模块

  **1. data_clean.py, 数据清洗**  
  * 剔除存在ST标记的数据和上市不满一年的股票
  * MAD法去除异常值，Z-值标准化，然后对行业哑变量和对数市值回归取残差，得到行业与市值中性化的因子值

  **2. factor_test.py, 单因子有效性的评价指标** 

（1）**因子收益率**：

均值、标准差 

$$R_{it} = \beta_{0t} + \beta_{1t} * f_{it}$$

（2）**因子收益率的T值**：

均值、标准差、大于0的概率、大于2的概率

（3）**IC值**：

均值、标准差、大于0的概率、大于0.02的概率、IC的IR

（4）**分层测试**

3. **因子打分标准**

对因子进行5项评估，满足标准得1分，不满足得0分，选取得分大于3的因子（也可以进一步考虑因子$IC$相关性，要求相关系数小于0.5，高相关性的因子以ICIR排序

| 指标                        | 打分标准（绝对值）            |
| --------------------------- | ----------------------------- |
| 最近60个月因子收益率均值    | $\bar r_f>0.002$              |
| 最近60个月因子收益率 $T$ 值 | $T>2$                         |
| 信息系数                    | $IC>0.02$                     |
| 基于$IC$的信息比率          | $IR>0.2$                      |
| 单调性得分                  | $\frac{R_5-R_1}{R_4-R_2} > 2$ |

4. **分档打分选股**

将股票按因子值排序，分为$N$档，第一档$N$分，第二档$N-1$分，依次类推，最后一档$1$分，对股票每个因子均进行排序，得到综合得分。