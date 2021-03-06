# “找卡卡”小程序

# 介绍文档

###### 2020 年 06 月 07 日

## 目 录

- 一、 作品综述...........................................................................................
   - 1 1 小程序说明....................................................................................
   - 1 2 需求分析........................................................................................
         - 1 2 1 应用场景..............................................................................
         - 1 2 2 功能结构..............................................................................
   - 1 3 解决的实际问题............................................................................
- 二、 技术开发方案...................................................................................
   - 2 1 小程序端........................................................................................
      - 2 1 1 总体设计和素材说明...........................................................
      - 2 1 2 页面设计................................................................................
      - 2 1 3 亮点——登录检测................................................................
   - 2 2 后台服务器端................................................................................
      - 2 2 1 后端系统结构........................................................................
      - 2 2 2 数据库设计............................................................................
      - 2 2 3 亮点——校园卡提醒............................................................
      - 2 2 4 难点——搜索查询................................................................
- 三、 团队情况...........................................................................................


## 一、 作品综述...........................................................................................

### 1 1 小程序说明....................................................................................

###### [ 注：本次提交的小程序为体验版。 ]

###### 找卡卡”小程序是针对高校开发的一款失物招领小程序。基于校园易发的

###### 失物现象，本产品提供了公开、便捷的信息平台；对于校园卡等重要证件的找回

###### 给予额外反馈，保障学生的校园生活质量。

###### 基本功能示意图


### 1 2 需求分析........................................................................................

###### 目标用户：高校学生

##### 1 2 1 应用场景..............................................................................

###### A) 寻找丢失物品：当用户发现丢失物品时，可以通过发布遗失信息，或者

###### 浏览待招领物品的方式寻找物品；当匹配成功时，该用户可查看拾得对应物品的

###### 用户联系方式。

###### B) 招领拾得物品：当用户发现遗失的物品时，可以通过发布招领信息，或

###### 者浏览丢失物品的方式寻找失主；当匹配成功时，该用户可查看丢失对应物品的

###### 用户联系方式。

###### C) 校园卡拾得提醒：特别地，当用户校园卡被招领时，“找卡卡”将以邮

###### 件方式通知用户，并附上拾取者的联系方式。

##### 1 2 2 功能结构..............................................................................

###### 功能结构图

### 1 3 解决的实际问题............................................................................

###### 高校人员相对固定，但校园范围大、人流多，很容易发生学习生活用品、甚

###### 至重要证件的丢失。“找卡卡”提供了失物招领的公共信息平台，方便用户进行

###### 信息交互，可有效提高校学生找回失物的概率。


## 二、 技术开发方案...................................................................................

### 2 1 小程序端........................................................................................

#### 2 1 1 总体设计和素材说明...........................................................

我们采用了三个tabbar作为功能的分区，绝大多数的元素采用小程序官方
推荐的flex布局方式，进行相关css的设置。元素大多采用rpx单位进行相关设
置，从而便于进行移动端的定位布局。

Npm板块设计 开源UI库vant
原型设计 墨刀软件
功能分区 tabber× 3 （失物、招领、我的）
图标来源 icon-font
布局方式 flex
元素定位 rpx单位

#### 2 1 2 页面设计................................................................................

###### 该小程序一共设计了 8 个页面，秉持简洁为美的设计理念。该小程序的所有

###### 网络请求均设计了较为优秀的提示加载，出错和成功信息，优化了用户体验。

###### 1 ) 丢失搜索成功反馈 2 )招领搜索失败反馈 3 )我的


###### 4 ) 信息完善 5 )我的招领 6 )我的丢失

###### 7 )帮助界面 8 )客服会话


#### 2 1 3 亮点——登录检测................................................................

###### 为了避免用户在未登录情况下发送错误表单数据，采用了针对登录状态的检

###### 测，未登录状态下必须先进行登录，否则无法查看相关信息的详情，无法进行发

###### 表。

我们将登录状态保存到app.globalData，以获取全局登录状态；同时为了降
低网络波动的影响，我们在页面的onReady生命周期中采用了定时器和递归的方
法。

### 2 2 后台服务器端................................................................................

#### 2 2 1 后端系统结构........................................................................

###### 整体架构 B/S

```
后端框架 django
服务器环境 centos 7. 3
数据库 MySQL
与前端连接 api接口和http协议
```
#### 2 2 2 数据库设计............................................................................

###### 表名 描述 主键

```
student 记录用户信息 student.id
event 发布的失物/招领消息 event.id
```

###### 数据库结构

#### 2 2 3 亮点——校园卡提醒............................................................

###### 当有人发布校园卡拾取信息时，我们不仅会发布当前消息，同时还会查找数

据库是否存在该同学的信息，如果能查到，则会利用python的SMTP简单邮件传
输协议，通过qq邮箱的接口向对应的同学邮箱发送邮件直接提醒到同学，从而
缩短寻找校园卡的时间。

###### 校园卡拾取提醒

#### 2 2 4 难点——搜索查询................................................................

###### A) 日期查询：正则表达式

###### 通过正则的方式可以匹配对应日期的消息记录，且从符号，数字，年月日的

###### 多种用户的输入习惯都进行了考虑。


###### 日期查找

```
B) 短文本匹配：jieba分词+TF-IDF算法。
```
jieba分词（https://github.com/fxsjy/jieba）是一个Python 中文分词
组件，提供对中文文本进行分词、关键词抽取等功能，支持三种分词模式：精确
模式（精确切分）、搜索模式（精确切分+长词切分）和全模式。其中前两种在
“找卡卡”中得以应用。

TF-IDF（词频-逆文档频率算法）是一种统计方法，用于评估一字词对一文
件集或一语料库的中的某一篇文档的重要性，字词的重要性随着它在文件中出现
的次数成正比增加，同时会随着它在语料库中出现的频率成反比下降。
利用jieba分词的搜索模式，我们将输入的文本转化为一组词向量，然后将
所得list与数据库中的记录进行匹配。基于集合的思想，对每个词进行计算和
归类，计算两个文本的词向量余弦相似度，以表示其近似程度，并挑选出相似度
大于 70 %的语句返回给前端，从而实现模糊匹配。


###### TF-IDF算法流程图

###### 其中词频为句子中的每个分词在词向量超集中出现的频数。

###### IDF权重计算公式如下：

```
IDFqi =log
```
###### N+ 0. 5

n(qi)+ 0. 5
N为总文本的分词数量，n(qi)为分词qi出现的频数，该权重表明词频越低，
权重越大，越能够代表该文本。其中 0. 5 为为了防止分母为 0 人工设置的参数。
两词向量的余弦相似度计算公式如下：

```
similarity=cos(θ)=
```
###### A∙B

###### ||A||||B||

```
= i=^1
```
```
n A
 i×Bi
```
```
i= 1
```
```
n (A
 i)^2 × i= 1
n (B
 i)^2
```

