# 毕业设计
毕业设计-汉语多音字注音研究
## 调研阶段

查了一些国内外的论文，总结了一下其中用到的方法

见《问题认识》.md

## 数据处理

用到的数据见Code里的data目录。

`pinyin.txt` ：汉字拼音库，汉字数：41450， 多音字个数：8570，来自[pinyin-data](https://github.com/mozillazg/pinyin-data)

`polyphones.txt`：将汉字拼音库中的多音字全部提取出来，存入该文件中

`polyphones.json`：将所有多音字和读音存储成json文件

`198801.txt`：人民日报1988年一月的新闻语料，一共有19374条新闻，包含5666个多音字，其中有89种不同的多音字，来自[pkuopendata](http://opendata.pku.edu.cn/dataset.xhtml?persistentId=doi:10.18170/DVN/SEYRX5)

`198801output.txt`：存储每个多音字出现的次数，按出现频次从多到少排序

`news.txt`：将所有包含多音字的新闻全部提取出来，存入该文件

`phrase.txt`：带注音的中文短语，一共有96809条，来自[ChineseTone](https://github.com/letiantian/ChineseTone)

`poly_phrase.txt`：将所有含多音字的短语输出到该文件中，共有73574条含多音字的短语。每条短语前面的数字是多音字在短语中的位置（从0开始计算）

`phrase_frequency.txt` ：输出包含多音字的短语中所有多音字出现的次数，按出现次数从多到少排序。

## 模型搭建

目前想到用两种方法搭建模型。

两种方法模型大体一致，但在将lstm的输出喂进全连接层时的输入不一样。

>  说明：代码中的各种参数并不是最优的参数，只是一个示例。

### 按照多音字的读音的分类方法

这种方法将训练数据按照其中某个多音字的读音进行分类，如训练数据中含有多音字“都”的句子将分为以下两类：

1. 句子中“都”的读音为`dū`的数据
2. 句子中“都”的读音为`dōu`的数据

此方法最大的弊端就是需要对每个多音字单独训练一个模型并测试，另外当一句话中含有两个及以上的多音字时，该方法并不能完成所有多音字的注音。

## 仿照词性标注问题的分类方法

代码：`code/PolyphoneDisambiguation/disambiguation_pos.py`

这种方法仿照了词性标注的处理思路，将训练数据编码为如下格式：

> ('在古都西安', '都', ['NA', 'NA', 'dū', 'NA', 'NA'])

其中“NA”代表非多音字，多音字则使用读音来编码。

