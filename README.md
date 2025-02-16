# HAN中文分词-2021 Spring NLP Homework

### 训练样本词频统计

使用`collection.Counter`和`nltk`相应工具包完成训练集词频统计分析。展示出现次数最多的前80个词。

![词频](./notebooks/前80个词的词频.png)

![词频-累加](./notebooks/前80个词的词频-累加.png)

完整的训练集词频统计信息见：[vocab-freq.txt](./results/vocab-freq.txt)。

### 结巴分词baseline

使用结巴分词默认配置（`jieba.cut`）得到结果于文件[jieba-test-result.txt](results/jieba-test-result.txt)中，执行测试脚本。

```bash
perl scripts/score datasets/training_vocab.txt datasets/test.txt results/jieba-test-result.txt
# 或者直接运行runeval.sh
```

示例代码：
```python
def write_test_result():
    with open(RESULT_FILE_TEST_IM, 'w+', encoding='utf-8') as f:
        f.writelines(['  '.join(jieba.cut(line)) + '\n' for line in test_raw])
```

得到结果：

```
RECALL: 0.787
PRECISION:      0.853
F1 :    0.818

OOV Rate:       0.058
OOV Recall:     0.583
IV Recall:      0.799
```


### 最大后向匹配

首先统计训练集词汇的长度分布，经观察得出，`max_length`选择4/5为最佳。

![词频分布-累加](notebooks/训练集词长度频率分布-累加.png)

示例代码：
```python
def max_back(line: str, max_len = 4):
    res = []
    n = len(line)
    idx = 0
    while idx < n:
        lens = max_len
        while lens > 0:
            sub = line[idx: idx + lens]
            # print(sub)
            if sub in vocab:
                res.append(sub)
                idx += lens
                lens = max_len
            else:
                lens -= 1
                if lens == 0:
                    idx += 1
    return res
```

```txt
RECALL: 0.903
PRECISION:      0.890
F1 :    0.896

OOV Rate:       0.058
OOV Recall:     0.000
IV Recall:      0.958
```

更多实验结果和测试报告见[result](./results/)和[logs](./logs/)文件夹。