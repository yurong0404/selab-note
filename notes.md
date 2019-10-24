## Tensorflow-gpu環境安裝 on Ubuntu 指南<br>
安裝深度學習環境需要有三樣東西，tensorflow、cuda和cudnn，cuda或tensorflow哪個先裝並不影響，但基於某些原因，我選擇先裝tensorflow。<br>
1. 首先確保你還沒有安裝tensorflow或tensorflow-gpu，有時keras也會影響tensorflow的套件，因此先把他們通通解安裝一遍，確保你沒安裝。這裡我假設你已經有python3和pip安裝工具。<br>

```console
$ python3 -m pip uninstall keras
$ python3 -m pip uninstall tensorflow
$ python3 -m pip uninstall tensorflow-gpu
```
2. 開始安裝tensorflow-gpu(如果你要用GPU訓練模型就必須裝tensorflow-gpu，但如果你想用CPU跑的話，就裝tensorflow或tensorflow-gpu都可以)，如果你有指定的tensorflow版本就後面加'=='和版本編號<br>
```console
$ python3 -m pip install tensorflow-gpu==  # 如果你想看有哪些版本編號，可以這樣查
$ python3 -m pip install tensorflow-gpu   # 如果沒特別指定版本，就這樣裝，自動安裝最新的版本
$ python3 -m pip install tensorflow-gpu==1.14.0   # 如果你想指定版本
```
3. 裝完後還需要安裝cuda toolkit和cudnn，但cuda toolkit和cudnn的版本必須裝相容於你安裝的tensorflow版本，網路上說明tensorflow對應的cuda、cudnn版本的文章五花八門，有的還不一定正確，最直接查詢的方法如下，你需要的版本讓你自己的電腦來告訴你<br>

```console
>>> import tensorflow
ImportError: libcublas.so.9.0: cannot open shared object file: No such file or directory
```
此時若你沒安裝cuda，必定會跳出錯誤訊息，從錯誤訊息中尋找如上的訊息：<br>
此行代表你需安裝cuda 9.0版本，於是就去 https://developer.nvidia.com/cuda-toolkit-archive 下載安裝檔<br>

4. 安裝cuda安裝檔前，必須要先裝好你的顯示卡driver，然而裝cuda會有規定你的顯卡驅動必須在某個版本以上，請從 https://tech.amikelive.com/node-930/cuda-compatibility-of-nvidia-display-gpu-drivers/ 這邊查看你要裝的cuda版本顯卡驅動最低要求是多少。<br><br>
如果你不知道你的顯卡驅動版本是多少的話，而且你GPU是NVIDIA的話，就下這指令:
```console
$ nvidia-smi
```

  

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
