# 基于Seq2Seq的中文分词



# 1. 使用效果

## 1.1 输入

输入待分词的句子或文章，如：

> 黑夜给了我黑色的眼睛，我却用它寻找光明。

## 1.2 输出

返回分词结果：

> 黑夜/给/了/我/黑色/的/眼睛/，/我/却/用/它/寻找/光明/。



# 2. 使用过程

将分词视作一个**序列标注**任务。

1. 训练数据预处理：对已经标注好分词的训练数据加上4元组标注['**B**egin', '**M**iddle', '**E**nd', '**S**ingle']，分别表示一个词语的开始、中间、结尾以及单字词语。
2. 模型训练：训练Seq2seq模型，保存模型。
3. 模型预测：加载模型，对测试语句进行序列标注，当得到标注为E或者S，则在该字符处切割。

# 3. 模型设计

典型的Seq2Seq结构，包含一个编码器Encoder，一个解码器Decoder。



# 4. 数据预处理

1. 字符频度统计，得到char_count_dict: {char: count}。

2. 编码器Encoder：建立字符字典，根据字符频度过滤低频度字符；建立字符词典char2id_dict：{char: index}。预留两个index：

   （1）为序列补齐，预留index=0。后续在批量模型训练时，数据需要相同长度，如果序列实际长度不足，可以通过在序列后面补pad字符将序列对齐，在计算loss的时候将pad字符所在位置mask掉，不计算这部分字符的loss。

   （2）为未知字符'<unk>'预留index=1。

3. 解码器Decoder：4-tag序列标注，加上2个预留字符，编码器字典是: {0: pad, 1: <start>, 2: 'B', 3: 'M', 4:'E', 5: 'S'}

   建立字符到index的映射，不在字典中的字符index=0。每一行是相等字符的字符index序列与标注序列的index。如：

   > 101,8,890,234,7 '\t'  52345    （SBMES）

   在训练数据生成过程中，给标注序列自动加上<start>。

4. 解码器Decoder：4-tag序列标注，加上2个预留字符，编码器字典是: {0: pad, 1: <start>, 2: 'B', 3: 'M', 4:'E', 5: 'S'}

   建立字符到index的映射，不在字典中的字符index=0。每一行是相等字符的字符index序列与标注序列的index。如：

   > 101,8,890,234,7 '\t'  52345    （SBMES）

   在训练数据生成过程中，给标注序列自动加上<start>。



# 5. 模型测试

# 6. 模型评估

# 7. 总结

# 8. 参考资料

