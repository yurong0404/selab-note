## Tensorflow-gpu環境安裝指南<br>
1. 首先確保你還沒有安裝tensorflow或tensorflow-gpu<br>
<console>
  

## Attention layer<br>
video: https://youtu.be/oaV_Fv5DwUM<br>
> Attention用在model能夠根據input自行調整input token的權重，做法為多加一層layer訓練給予input的權重。<br><br>
例如：要訓練一個輸入句子，輸出sentiment為positive或negative的model，input有"I like it"、"I dislike it"、"It is awful"，
到底該給第幾個token比較高的權重？固定給某個位置的token比較高的權重很不靈活，應該根據input本身自行決定第幾個token擁有高權重，
因此多給一個attention layer輸入input tokens，輸出input tokens的權重，用該權重與input tokens相乘，才往後面的layer繼續運算，
這做法才夠彈性去預測該句子是positive或negative。

## Dense layer<br>
> dense意思為濃密的意思，顧名思義dense layer意思是就是把維度大的input輸出成維度小的output，把維度擠起來dense起來的感覺<br>
例如: 將長度1024的array經過一層dense layer轉成長度64的array。要把輸入最終分成64類的話，最後一層的dense layer輸出就會是長度64。
然後再接到softmax層，把那64個-1~1的數字，轉成64個數字加起來為1的分布機率。

## Softmax<br>
> 把輸入為不是機率分布的array，轉成是機率分布的array。<br>
例如：長度為64的array數值都介於-1到1，經過softmax應變為，64個range介於0到1的值，且64個數字相加為1。
