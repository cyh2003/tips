Basic
-----------

### Sequence

1.  一般而言模型不会对输入语句的长度和`batch size`做出限制

### Encoding

1.  `BPE`粒度介于字符级表示和单词级表示之间
2.  `BPE`弥补了字符级表示的效果不够好和单词级表示出现`OOV`的问题
3.  注意中英文序列的编码有可能出现一个`token`对应多个字符或单词

### Loss

1. 将`label`置为`-100`的`token`不参与`loss`的计算

Model
-----------

### Transformer

1.  `Transformer`通过`self-attention`实现了单个语句训练时（相对`RNN`）`encoding`的并行，通过`teacher forcing`和`sequence mask`实现了单个语句训练时（相对`RNN`）`decoding`的并行，相应的，`Transformer`在测试时`decoding`只能串行
2.  `Transformer`测试时通过使用`beam search`可以给出多个候选翻译语句，提高了给出较优结果的概率

Task
-----------

### Language Model

1.  语言模型任务是指给定一段文本预测下一个词（`token`）的任务

Criterion
-----------

### BLEU

1. 用于评价翻译模型的效果，满分为`1.0`

Framework
-----------

### Fairseq

1. 训练时默认每个`epoch`进行一次`validation`，并默认保存每个`epoch`结束时的状态以及单独保存结束的状态和最好的状态
2. 当训练任务因意外中断时，重新执行命令即可实现断点重续的功能

### Huggingface Transformers

1. 用于评价翻译模型的效果，满分为`1.0`
