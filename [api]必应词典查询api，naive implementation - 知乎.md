# [api]必应词典查询api，naive implementation - 知乎
最近上课想给学生做一下单词表，以前有学生反映课本的单词表没有音标，这也确实是一个问题。而且以前的单词表单词和解释之间没有明显界限，不适合做成表格，更不要说生成单词卡了。所以要是能有一个词典 api，我出一个单词，它告诉我音标，解释，例句就好了。

[Jonathan LEI：必应词典第三方 API v2.0​zhuanlan.zhihu.com![](https://pic2.zhimg.com/3df4da33f3a2b062f8c7aceaa8bcfbed_180x120.jpg)
](https://zhuanlan.zhihu.com/p/22421123)

上面这个是在知乎上别人做的 api，在线那种。不过现在上不去。

所以我就想，要不自己做一个吧。

* * *

目前需求比较简单（其实主要是想自动生成音标，然后查找比较公认的解释）。微软词典本身没有做 api，所以现在只能模拟网页访问，然后 parse 返回的 html 页面。不过有了 requests 和 beautifulsoup，这些都不是事。

## 需要的 python package

如果还没有安装 requests 和 beautifulsoup 的话，使用下面的命令安装，可以带`-i https://pypi.tuna.tsinghua.edu.cn/simple`这个参数，会加快安装速度

```bash
pip install requests
pip install beautifulsoup4
```

导入会用到的包

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd
import time
```

## DictRstParser 类

不确定应该怎么写的时候，先弄个类出来总不会有大错。

大体来说这个 naive implementation 的步骤包括：

1.  使用 requests.get 从**`https://cn.bing.com/dict/search?q=`**拿到某个词在词典的查询结果网页。
2.  利用 beautifulsoup 把网页文本转换成方便查找信息的 soup 形式
3.  利用浏览器的`检查`工具，找到感兴趣的内容所在的 html 标签
4.  使用 select 或者 find 得到这个标签，然后用 get_text() 函数获得里面的文本

这是最开始的设计，后面把 4 这步抽象成了一个函数，加入了情况判定:

```python
    def get_content(self,from_obj,selector_str):
        rst = from_obj.select(selector_str)
        if len(rst)==0:
            return None
        elif len(rst)==1:
            return rst[0].get_text().replace("\xa0"," ").strip()
        else:
            return '\n'.join([e.get_text() for e in rst])
```

然后就是不停的使唤这个函数了，不过复制粘贴的多了（当你粘贴次数超过三次的时候，就应该想一下自己是不是代码写的有点不对了），觉得使用 dict 会更好一点，目前`__init__`函数长这样：

```python3
    def __init__(self,word_str):
        self.req_link = f"https://cn.bing.com/dict/search?q={word_str}"
        self.res_txt = requests.get(self.req_link).text
        self.soup = BeautifulSoup(self.res_txt,'html.parser')
        self.word_info_selector_dict = {
            'word':'div#headword',
            'eng_pr':'div.hd_pr',
            'ame_pr':'div.hd_prUS',
            'tongyi':'div.wd_div',
            'fushu':'div.hd_div1',
            'defination':'div.qdef > ul > li',
        }
        self.word_info_dict = {}
        self.related_divs = self.soup.select('div.lf_area > div')
        for k,v in self.word_info_selector_dict.items():
            self.word_info_dict[k]=self.get_content(self.related_divs[0],v)

        self.word_info_dict['sentences']=self.get_content(self.related_divs[1],'div#sentenceSeg')
```

主要的配置部分写在 word_info_selector_dict 里面，然后通过遍历 word_info_selector_dict .item() 来实现反复使唤 get_content 的目的

## 试试这个 api

现在的工作流程是将要找的单词放在一个单独的文本文件里，比如说`wordlist.txt`，然后在代码里面像下面这么做：

```python
with open('wordlist.txt','r') as wordlistfile:
    word_list = wordlistfile.readlines()

row_list = []

for w in word_list:
    parser = DictRstParser(w)
    row_list.append(parser.word_info_dict)
    time.sleep(0.5)

df = pd.DataFrame(row_list)

xlwriter = pd.ExcelWriter('wordlist.xlsx',engine='xlsxwriter')
df.to_excel(xlwriter,'Sheet0')
xlwriter.save()
```

前面是读取`wordlist.txt`这个文件，然后 readlines 把各行弄进一个 list 里面。之后遍历这个 list，生成 DictRstParser 实例，读取做好的 word_info_dict。为了防止被封，每走一个单词停半秒 (time.sleep(0.5)）

输出的时候用 xlsx 而不是 csv：毕竟转码是好麻烦的一件事情，尤其是这种中英混搭还有音标的，天知道该用什么码。总之交给 excel 自己搞定好了

![](https://pic3.zhimg.com/v2-0c50c28b321176d80b59a593206f65d2_b.jpg)

输出结果如图

需要 note 的一下的地方：**如何逐行累积最后形成一个 dataframe**[\[1\]](#ref_1)：

1.  生成一个空的 list：`row_list =[]`
2.  将各行内容做成 dict，逐个往 list 里 append: `row_list.append(parser.word_info_dict)`
3.  使用 pandas.DataFrame() 将 list 转成 df: `df = pd.DataFrame(row_list)` [\[2\]](#ref_2)

## 参考

1.  [^](#ref_1_0)stackoverflow 上的相关讨论 [https://stackoverflow.com/questions/10715965/add-one-row-to-pandas-dataframe](https://stackoverflow.com/questions/10715965/add-one-row-to-pandas-dataframe)
2.  [^](#ref_2_0)pd 是前面导入的时候用了 import pandas as pd，这还是蛮常见的一种写法 
    [https://zhuanlan.zhihu.com/p/82901593](https://zhuanlan.zhihu.com/p/82901593)
