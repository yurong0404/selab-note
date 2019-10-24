## Tensorflow-gpu環境安裝 on Ubuntu 指南<br>
安裝深度學習環境需要有三樣東西，tensorflow、cuda和cudnn，cuda或tensorflow哪個先裝並不影響，但基於某些原因，我選擇先裝tensorflow。<br>

### Step 1
首先確保你還沒有安裝tensorflow或tensorflow-gpu，有時keras也會影響tensorflow的套件，因此先把他們通通解安裝一遍，確保你沒安裝。這裡我假設你已經有python3和pip安裝工具。<br>
(小知識：請不要重複安裝tensorflow的各種不同版本，很容易出問題，請一層一層解安裝完，)<br>

```console
$ python3 -m pip uninstall keras
$ python3 -m pip uninstall tensorflow
$ python3 -m pip uninstall tensorflow-gpu
```
### Step 2
開始安裝tensorflow-gpu(如果你要用GPU訓練模型就必須裝tensorflow-gpu，但如果你想用CPU跑的話，就裝tensorflow或tensorflow-gpu都可以)，如果你有指定的tensorflow版本就後面加'=='和版本編號<br>
```console
$ python3 -m pip install tensorflow-gpu==  # 如果你想看有哪些版本編號，可以這樣查
$ python3 -m pip install tensorflow-gpu   # 如果沒特別指定版本，就這樣裝，自動安裝最新的版本
$ python3 -m pip install tensorflow-gpu==1.14.0   # 如果你想指定版本
```
### Step 3
裝完後還需要安裝cuda toolkit和cudnn，但cuda toolkit和cudnn的版本必須裝相容於你安裝的tensorflow版本，網路上說明tensorflow對應的cuda、cudnn版本的文章五花八門，有的還不一定正確，最直接查詢的方法如下，你需要的版本讓你自己的電腦來告訴你。<br>

```console
>>> import tensorflow
ImportError: libcublas.so.10.0: cannot open shared object file: No such file or directory
```
此時若你沒安裝cuda，必定會跳出錯誤訊息，從錯誤訊息中尋找如上的訊息：<br>
此行代表你需安裝cuda 10.0版本，於是就去 https://developer.nvidia.com/cuda-toolkit-archive 下載安裝檔，這我這邊是載runfile的版本。<br>
要是你沒有跳錯誤訊息，大概是tensorflow想用CPU跑，所以讓你過了，那你就在python shell用下面這個指令硬是trigger一下GPU，他就會跳錯誤訊息了。<br>
```console
>>> sess = tf.Session(config=tf.ConfigProto(log_device_placement=True))
ImportError: libcublas.so.10.0: cannot open shared object file: No such file or directory
```
### Step 4
安裝cuda安裝檔前，必須要先裝好你的顯示卡driver，然而裝cuda會有規定你的顯卡驅動必須在某個版本以上，請從 https://tech.amikelive.com/node-930/cuda-compatibility-of-nvidia-display-gpu-drivers/ 這邊查看你要裝的cuda版本顯卡驅動最低要求是多少。<br><br>
如果你不知道你目前的顯卡驅動版本是多少的話，而且你GPU是NVIDIA的話，就下這指令就可以查看你的driver版本，像我的是410.79。<br>
```console
$ nvidia-smi
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 410.79       Driver Version: 410.79       CUDA Version: 10.0     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce GTX 1080    Off  | 00000000:01:00.0  On |                  N/A |
|  0%   44C    P8    15W / 200W |   8062MiB /  8116MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
|   1  GeForce GTX 1080    Off  | 00000000:03:00.0 Off |                  N/A |
|  0%   48C    P8    13W / 200W |    123MiB /  8119MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|    0     12590      C   /usr/bin/python3                            7647MiB |
|    0     29006      G   /usr/lib/xorg/Xorg                           192MiB |
|    0     29572      G   compiz                                       165MiB |
|    0     30886      G   ...uest-channel-token=14735455047927048595    45MiB |
|    1     12590      C   /usr/bin/python3                             111MiB |
+-----------------------------------------------------------------------------+
```
如果說你還沒有裝GPU的driver或你的driver版本太舊的話，也沒關係，cuda的安裝檔執行後一開始就會先問你要不要幫你裝顯卡驅動，你可以選yes，他就會自己幫你裝好對的驅動版本，如果你本來的驅動程式版本就夠新就選no。<br>
這邊要特別注意，如果你有要裝顯卡驅動的話，你要先暫時跳到tty的環境裝cuda和顯卡驅，ctrl+alt+f1跳到tty1，否則你在GUI的介面安裝會fail。<br>

====== 如果你要裝驅動，先執行這段，如果沒有要裝驅動，就可以直接執行cuda安裝檔 =======<br>
```console
$ sudo apt-get purge nvidia*  # 如果你顯卡驅動版本太舊，先執行這行刪除你原本的驅動，如果最後訊息有跳出autoremove什麼的，就照他說的執行。
$ sudo service lightdm stop   # 裝顯卡驅動前，請必先照這指令執行，這指令在關掉 X server，別問我 X server是什麼，應該是跟螢幕顯示相關的東東
```
========= 以上做完再執行cuda安裝檔 ==============<br>
```console
$ sudo sh ./cuda_10.0.130_410.48_linux.run   # 執行你的cuda安裝檔
```
安裝過程，他大概會問你要不要建link路徑，選yes或no都沒差，還有安裝CUDA sample什麼的，不裝也沒差。<br>
你裝完後就可以從tty1跳回GUI介面了，ctrl+alt+f7跳回GUI。裝完他最後的訊息應該有叫你要加cuda的資料夾到環境變數，請到~/.profile或者/etc/profile擇一檔案最底下加入這兩行，如果你cuda版本不是10.0就自己改一下。<br>
```console
export PATH=$PATH:/usr/local/cuda-10.0/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-10.0/lib64
```
加完後，在terminal執行它一下<br>
```console
$ source ~/.profile
$ source /etc/profile
```
到這，沒意外的話，你算安裝cuda成功了。

```console
$ python3
>>> import tensorflow
```
### Step 5
安裝好cuda後，開始安裝cudnn。首先，測試一下，還有沒有跳出 ImportError: libcublas.so.10.0: cannot open shared object file: No such file or directory的error，如果沒有，代表你cuda安裝成功。但沒意外的話你會改跳 ImportError: libcudnn.so.7: cannot open shared object file: No such file or dictionary 這樣的error，這意思是說你要裝cudnn 7的版本，到這邊隨便下載cudnn 7.X for cuda 10.0的檔案 https://developer.nvidia.com/rdp/cudnn-archive ，但當然如果你需要的cudnn不是7或是cuda不是10.0，那你就自己變通一下載你要的版本，下載前必須註冊登入NVIDIA帳號，沒帳號就安裝一下。<br><br>

載好cudnn後很簡單，就像下面這樣安裝<br>
```console
$ tar -xzvf cudnn-9.0-linux-x64-v7.tgz
$ sudo cp cuda/include/cudnn.h /usr/local/cuda/include
$ sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
$ sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```
裝完後理論上就好了，測試一下你能不能import tensorflow了吧
```console
$ python3
>>>import tensorflow
```
如果還不能import，就見鬼了。<br>
最後，執行這個確認你的tensorflow可以用GPU跑<br>
```console
>>> import tensorflow as tf
>>> tf.test.is_gpu_avaiable()
```
如果回傳true的話，恭喜你能用gpu跑tensorflow了，那你可以關掉這個筆記了<br>
如果你很不幸，這函式回傳false的話，那真的見鬼了這樣，你也能關掉這筆記了另求解了，因為我不知道怎幫你QQ。<br>
  

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
