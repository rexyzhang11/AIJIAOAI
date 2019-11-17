```python
import jieba 
from gensim.models import word2vec
import json
import random
f1 = open("fenci.txt",encoding='utf8')
f2 = open("fenci_result.txt","a",encoding='utf8') #储存分词结果
lines = f1.readlines()
for line in lines:
    line.replace("\t","").replace("\n","").replace(" ","")#去换行、tab、和空格
    seg_list = jieba.cut(line)
    seg = " ".join(seg_list)
    f2.write(seg) #分词用空格隔开
f1.close()
f2.close()

#加载语料库
sentences = word2vec.Text8Corpus("fenci_result.txt")
model = word2vec.Word2Vec(sentences,min_count=1)
model.save("wordToVec.model")
with open("content_file.txt",'r',encoding="utf8") as f:
    content_file = json.load(f)

user_sentence = input("请输入一段文本（输入退出即可退出）:\n")    
while user_sentence != '退出': 
    user_fenci = list(jieba.cut(user_sentence))
    max_similarity = 0 
    max_title = ""
    max_reply = []
    for n in content_file:
        question_fenci = list(jieba.cut(n['title']))
        try :
            similarity = model.wv.n_similarity(user_fenci,question_fenci)
        except:
            continue #异常跳过
        if similarity > max_similarity:
            max_title = n['title']
            max_reply = n['reply']
            max_similarity = similarity
    print('匹配问题:', max_title)
    print('匹配相似度',max_similarity)
    if len(max_reply) > 0:
        print(max_reply[random.randint(0,len(max_reply)-1)]) #随机选一个
    else:
        print("还不会，待学习~")
    user_sentence = input("请输入一段文本:\n")

```

    Building prefix dict from the default dictionary ...
    Loading model from cache C:\Users\rexyz\AppData\Local\Temp\jieba.cache
    Loading model cost 0.523 seconds.
    Prefix dict has been built succesfully.
    

    请输入一段文本:
    睡了没
    匹配问题: 三天没抽烟了，表扬我😎
    匹配相似度 0.8488764
    各位都是我戒烟路上的贵人😊
    请输入一段文本:
    睡了没
    匹配问题: 三天没抽烟了，表扬我😎
    匹配相似度 0.8488764
    各位都是我戒烟路上的贵人😊
    请输入一段文本:
    快夸我
    匹配问题: 夸我不然憋说话！
    匹配相似度 0.88925755
    lz效率好高啊！（就是有点拖延症嘻嘻嘻）加油加油！答辩加油！
    请输入一段文本:
    快夸我
    匹配问题: 夸我不然憋说话！
    匹配相似度 0.88925755
    是
    请输入一段文本:
    快夸我
    匹配问题: 夸我不然憋说话！
    匹配相似度 0.88925755
    lz效率好高啊！（就是有点拖延症嘻嘻嘻）加油加油！答辩加油！
    请输入一段文本:
    退出
    


```python


```


```python

```
