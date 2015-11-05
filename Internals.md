Some internals


由于一些原因，此project可能不会有太多时间维护。在这里简单把一些内部的东西说说。这个修改版本最主要的工作实际上添加了一个中文分词的tokenizer。tokenizer就是把文本切分的东西，比如英文按照标点和空格分开，中文按照一定的算法分成要索引的词。sphinx原版中有针对单字节编码的tokenizer，也有针对utf-8编码的tokenizer，这个修改版本就是添加了采用MMSEG分词算法的tokenizer。

如果想看看具体改动哪些（这里以sphinx-for-chinese 1.10-r2287为例），首先去check out一份原版的sphinx代码：


svn checkout http://sphinxsearch.googlecode.com/svn/trunk/ sphinxsearch-read-only -r 2287


要确保原版的svn版本号也是2287，然后export一份。将这份代码，与sphinx-for-chinese 1.10-r2287进行比较。


diff sphinx-for-chinese sphinxsearch -r


就可以看到具体改动了哪些代码。改动实际上并不多。如果想自己升级，完全可以对比着修改一下，除非sphinx代码架构上有很大变化，一般直接copy代码到相应的部分即可。


添加的MMSEG算法，除了利用了darts-clone这个类库，另外一个主要的思路就是利用浮点数组保存trunks，然后用三层for循环去处理。这也是为什么分此速度快的原因。


另，sphinx原版的代码中有一些小bug，在一些特殊的应用场景里会有问题，目前已知的有内存属性回写问题，prefork的异常退出问题。我们对此进行了一些处理，由于某些原因这些处理并未集成到当前的sphinx-for-chinese版本。可以等待官方修复。