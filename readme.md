## 論文列表
***
* [1] A Framework for Software Defect Prediction and Metric Selection, 2017<br>
*Shamsul Huda et al.*<br>
paper: https://ieeexplore.ieee.org/abstract/document/8240899<br>
***

* [2] Learning a Metric for Code Readability, 2010<br>
*Raymond P. L. Buse and Westley R. Weimer*<br>
paper: https://web.eecs.umich.edu/~weimerw/p/weimer-tse2010-readability-preprint.pdf<br>
source code: http://www.arrestedcomputing.com/readability<br>
> Buse為程式可讀性分類模型之始祖，使用數個structural feature做為分類可讀性之依據，使用logistic regression訓練二元分類器，其搜集100份Java snippets之可讀性評分作為訓練用途。
***

* [3] A General Software Readability Model, 2011<br>
*Jonathan Dorn*<br>
paper: https://web.eecs.umich.edu/~weimerw/students/dorn-mcs-paper.pdf<br>
> Dorn使用傅立葉轉換的技巧幫助分類程式可讀性，其dataset大小為readability研究中最大，包含Python、Java、Cuda各120個snippets之人工標記評分。
***

* [4] Improving Code Readability Models with Textual Features, 2016<br>
*Simone Scalabrino et al.*<br>
paper: http://www.cs.wm.edu/~denys/pubs/ICPC'16-Readability.pdf<br>
dataset: https://dibt.unimol.it/icpc2016/appendix.html<br>
> 提出幾個textual feature做為度量software readability的依據，再加上Buse提出的feature，訓練成程式可讀性的分類器，使用Java file作為訓練dataset
***

* [5] Improving code readability classification using convolutional neural networks, 2018<br>
*Qing Mi et al.*<br>
paper: https://www.sciencedirect.com/science/article/pii/S0950584918301496<br>
source code: https://github.com/CityU-QingMi/DeepCRM<br>
> 第一位使用深度學習訓練程式可讀性之研究，此篇研究使用CNN，並使用CheckStyle和PMD兩款tool自動標記訓練資料(25000筆Java file)，為當前可讀性分類準確度最高，其使用token level、character level、和node level (abstract syntax tree)三種表示法做訓練。
***

* [6] code2vec: Learning Distributed Representations of Code, 2018<br>
*URI ALON et al.*<br>
paper: https://arxiv.org/pdf/1803.09473.pdf<br>
source code: https://github.com/tech-srl/code2vec<br>
demo: https://code2vec.org/ <br>
> 將source code透過深度學習模型，學習function內之語句內容，並預測該function名稱，其應用可用於大範圍之source code分析研究，例如可幫助工程師檢測function取名是否恰當，亦可幫助defect prediction、code completion等研究。其深度學習模型使用兩層full connected layer和attention機制。
***

* [7] Context2Name: A Deep Learning-Based Approach to Infer Natural Variable Names from Usage Contexts, 2018<br>
*Rohan Bavishi et al.*<br>
paper: https://arxiv.org/abs/1809.05193<br>
source code: https://github.com/rbavishi/Context2Name<br>
> Javascript領域有許多人會將source code做minification，將變數名稱縮短為無意義之名稱，並且將空格縮排全刪除，以確保達到縮小容量之目的，亦或是達到隱藏程式邏輯之目的，這研究使用深度學習學習source code之邏輯，並將無意義名稱之變數還原成有意義之名稱，以方便閱讀。在此領域之研究，以有JSNice、JSNaughty等還原工具。
***

* [8] Summarizing Source Code using a Neural Attention Model, 2016<br>
*Srinivasan Iyer et al.*<br>
paper: https://www.aclweb.org/anthology/P16-1195<br>
source code: https://github.com/sriniiyer/codenn<br>
> Code caption，使用深度學習之技術描述一段code在做什麼事情，使用RNN+attention+embedding，縮寫CODE-NN。Dataset使用C#和SQL，但大部分code都不完整，難以生成語法樹。
***

* [9] A Convolutional Attention Network for Extreme Summarization of Source Code, 2016<br>
*Miltiadis Allamanis et al.*<br>
paper: https://arxiv.org/pdf/1602.03001.pdf<br>
source code: https://github.com/mast-group/convolutional-attention<br>
> 使用CNN+Attention預測function名稱
***

* [10] Improving Automatic Source Code Summarization via Deep Reinforcement Learning, 2018<br>
*Yao Wan et al.*<br>
paper: https://arxiv.org/pdf/1811.07234.pdf<br>
> dataset使用[11]的dataset
***

* [11] A parallel corpus of Python functions and documentation strings for automated code documentation and code generation, 2017<br>
*Antonio Valerio Miceli Barone and Rico Sennrich*<br>
paper: https://arxiv.org/pdf/1707.02275.pdf<br>
dataset: https://github.com/EdinburghNLP/code-docstring-corpus<br>
> Yao Wan[10]使用此論文之dataset
***

* [12] Deep Code Comment Generation, 2018<br>
*Xing Hu et al.*<br>
paper: https://xin-xia.github.io/publication/icpc182.pdf<br>
dataset: https://github.com/xing-hu/DeepCom<br>
> Source code summarization，縮寫DeepCom，比CODE-NN強，使用了自創的方法描述AST，因為傳統travesal approach(pre-order、post-order)還原的AST無法唯一。使用Seq2seq model，encoder和decoder都是LSTM，word embedding和hidden state都是512維，使用BLEU-4度量生成的句子。Dataset很多重複data。
***

* [13] Automatic Source Code Summarization with Extended Tree-LSTM, 2019<br>
*Yusuke Shido et al.*<br>
paper: https://arxiv.org/pdf/1906.08094.pdf<br>
source code: https://github.com/sh1doy/summarization_tf<br>
> Source code summarization，比DeepCom和CODE-NN強，dataset取自DeepCom。這世界上除了LSTM之外，還有Tree-LSTM，它不像LSTM是那種sequence順序的，而是以tree的結構訓練，看一下論文裡的figure 2(c)就能明白。Tree-LSTM又依照他cell裡計算方式的不同分兩種，一種是child-sum Tree-LSTM，另一種是N-ary Tree-LSTM。但由於傳統Tree-LSTM不能handle tree中任意數量的子節點，所以作者提出Tree-LSTM的改良版，Extended Tree-LSTM，也稱為multi-way Tree-LSTM。最後的BLEU4結果，我不明白為何可以那麼低，只有0.2左右，我猜是他code沒寫好，因為DeepCom跟我自己訓練都至少0.35。他在dataset的部分一樣沒有解決DeepCom的dataset有重複的data的問題。
***

* [14] Recommendations for Datasets for Source Code Summarization, 2019<br>
*Alex LeClair, Collin McMillan*<br>
paper: https://www.aclweb.org/anthology/N19-1394<br>
data: http://leclair.tech/data/funcom/<br>
> 待讀
***

* [15] Does BLEU Score Work for Code Migration?<br>
*Ngoc Tran et al.*<br>
paper: https://ieeexplore.ieee.org/document/8813269<br>
> 待讀
***

* [16] METEOR: An Automatic Metric for MT Evaluation with Improved Correlation with Human Judgments<br>
*Satanjeev Banerjee and Alon Lavie*<br>
paper: https://www.aclweb.org/anthology/W05-0909.pdf
> 待讀
***

* [17] Automatic Documentation Generation via Source Code Summarization of Method Context<br>
*Paul W. McBurney and Collin McMillan*<br>
paper: https://www3.nd.edu/~cmc/papers/mcburney_icpc_2014.pdf
> 用非deep learning的方法生成function的註解，包含“描述程式碼的行為”、“該function什麼樣的function呼叫”、“被何種statement使用(if、for、while)”
***

* [18] Supporting Program Comprehension with Source Code Summarization <br>
*Sonia Haiduc et al.*<br>
paper: https://ieeexplore.ieee.org/document/6062165
> 用非deep learning的方法生出function的數個關鍵字，幫助工程師了解該function的用途
***

* [19] Towards Automatically Generating Summary Comments for Java Methods <br>
*Giriprasad Sridhara et al.*<br>
paper: https://dl.acm.org/doi/pdf/10.1145/1858996.1859006
> 用非deep learning的方法描述function的行為，然而該方法使用的工具已經連結失效
***

* [20] Generating Natural Language Summaries for Crosscutting Source Code Concerns <br>
*Sarah Rastkar et al.*<br>
paper: https://ieeexplore.ieee.org/document/6080777
> 主要針對一個project中的function，找出與該function具有耦合性的物件，並用一篇summary列出說明，如附檔圖一所示。其輸出的內容非描述function的行為，而是描述該function與哪些物件耦合
***

* [21] Natural Language Models for Predicting Programming Comments <br>
*Dana Movshovitz-Attias et al.*<br>
paper: https://pdfs.semanticscholar.org/4a90/8858ba9223289c3b3b1d5ceceb9c70a78d6e.pdf
> 目的為針對一個function註解進行auto completion，對寫到一半的註解，進行自動生成，以加速工程師撰寫註解的速度。例如某個function的註解為“Train a named-entity extractor”，當工程師撰寫到”Train a named-“時，該論文的工具可根據程式碼自動完成”Train a named-entity“，而工程師再次撰寫至”Train a named-entity ext“時，該論文的工具可自動完成”Train a named-entity extractor“
