# query-document-search-engine
项目目的：给定一个query，返回给相关的文档
记录整个项目中的总结

![Image text](https://github.com/yuqinyuqinyuqin/query-document-search-engine/blob/main/untitled.png)
文档存储在ES里，我们为文档的标题，关键词分别建立正排和倒排索引（用于term匹配）

召回层：分两路召回，一路为基于term匹配。一路为基于语义匹配
      基于语义的召回需要我们离线将doc的表征存储好，query一来只需在线获得query对应的表征，和doc的表征进行相似度计算。为了快速地检索到和query语义最相似的向量，需要用最近邻算法来快速返回距离query最近的doc向量【目前调查有谷歌的FAISS 和ANNOY 很可！】

召回层返回给我们一些文档(数量级在百），我们需要再对其进行排序返回跟query最相关的doc（数量级在十）
排序层：不同于电商的精排，我们采用树模型LightGBM，初步考虑的特征有 静态特征（关于DOC的，可离线计算好），动态特征(QUERY 与DOC 相交互的），深度Bert计算的query和doc的相似度作为一个重要特征。

关于后续是返回给文档 还是query在文档中对应的答案，后面进行query-passage的匹配
