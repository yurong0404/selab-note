# python中套件的路徑問題
因為在實驗室每次在使用pip安裝套件時，很常就遇到Permission denied的error，然後也搞不懂到底要怎妥善的管理OS底下各個使用者所用的python，以下說明一次搞清楚！！<br>
首先根據python3執行檔的所在路徑，我們可知python3是OS底下所有使用者共用<br>
```bash
$ which python3
/usr/bin/python3
```
在安裝套件時，常常就是很直覺的輸入以下指令(以安裝jieba套件為例)<br>
```bash
$ python3 -m pip install jieba
Processing ./.cache/pip/wheels/af/e4/8e/5fdd61a6b45032936b8f9ae2044ab33e61577950ce8e0dec29/jieba-0.42.1-cp35-none-any.whl
Installing collected packages: jieba
ERROR: Could not install packages due to an EnvironmentError: [Errno 13] Permission denied: '/usr/local/lib/python3.5/dist-packages/jieba-0.42.1.dist-info'
Consider using the `--user` option or check the permissions.

WARNING: You are using pip version 19.3.1; however, version 20.1.1 is available.
You should consider upgrading via the 'pip install --upgrade pip' command.
```
哇！結果如何？結果給你吃error啊！根據error message，看到Permission denied字眼，你可能心想指令前加個sudo就解決了，沒錯，如果你的帳戶是管理者帳戶，加sudo就可以正常安裝了。但你猜如何，加sudo安裝套件的話，server底下所有使用者使用python時、使用jieba套件時都要用你安裝的這個jieba版本啊！如果你今天是安裝tensorflow，tensorflow版本不同很容易一堆問題啊！不是每個使用者都想用你的tensorflow版本啊！所以每個使用者都能獨立使用屬於自己的python環境是很重要的！<br><br>

如果想要避免server的所有使用者在使用python時的套件衝突問題，我們應該要將所有使用者都設為非管理者帳戶。所有使用者安裝套件，都應該要安裝在自己帳戶的路徑底下，不應該要安裝在所有使用者都可以access到的路徑，這樣所有使用者就不會彼此干擾，自己的套件自己負責自己管理!<br><br>
## 安裝python套件在自己使用者的路徑下，標準作法如下
```bash
$ python3 -m pip install jieba --user
Processing ./.cache/pip/wheels/af/e4/8e/5fdd61a6b45032936b8f9ae2044ab33e61577950ce8e0dec29/jieba-0.42.1-cp35-none-any.whl
Installing collected packages: jieba
Successfully installed jieba-0.42.1
```
"Successfully installed"，漂亮！用這個指令套件會裝在"~/.local/lib/python3.5/site-packages/"這個路徑底下(我的python是3.5版，請自己改成你python的版本)<br>
```bash
$ ls ~/.local/lib/python3.5/site-packages/jieba
jieba/                  jieba-0.42.1.dist-info/ 
```
漂亮！jieba套件果然在這路徑底下，這樣我安裝套件就不會影響到其他使用者帳戶了。<br>
如果要刪除套件的話，不用加--user，然後jieba就從"~/.local/lib/python3.5/site-packages/"路徑底下移除了。<br>
```bash
$ python3 -m pip uninstall jieba
Uninstalling jieba-0.42.1:
  Would remove:
    /home/selab/.local/lib/python3.5/site-packages/jieba-0.42.1.dist-info/*
    /home/selab/.local/lib/python3.5/site-packages/jieba/*
Proceed (y/n)? y
  Successfully uninstalled jieba-0.42.1
```

## 安裝python套件在公共路徑下，做法如下
再來，我要示範套件安裝到所以使用者都會access到的路徑，加sudo就會裝到公共路徑"/usr/local/lib/python3.5/dist-packages/"底下了，如底下指令所示，但為了不影響其他使用者，請盡量別用以下方法做安裝套件<br>
```bash
$ sudo python3 -m pip install jieba
Processing ./.cache/pip/wheels/af/e4/8e/5fdd61a6b45032936b8f9ae2044ab33e61577950ce8e0dec29/jieba-0.42.1-cp35-none-any.whl
Installing collected packages: jieba
Successfully installed jieba-0.42.1
```
```bash
$ ls /usr/local/lib/python3.5/dist-packages/ | grep jieba
jieba
jieba-0.42.1.dist-info
```
從公共路徑刪除套件，作法如下，一樣就加sudo就好<br>
```bash
$ sudo python3 -m pip uninstall jieba
Uninstalling jieba-0.42.1:
  Would remove:
    /usr/local/lib/python3.5/dist-packages/jieba-0.42.1.dist-info/*
    /usr/local/lib/python3.5/dist-packages/jieba/*
Proceed (y/n)? y
  Successfully uninstalled jieba-0.42.1
```
## 檢查你過去安裝的套件路徑
你可能看完上面的說明，會開始好奇，我以前到底都把套件裝去哪了，以下為查看tensorflow的套件路徑範例(要先import才能看路徑)<br>
```bash
$ python3
Python 3.5.2 (default, Apr 16 2020, 17:47:17) 
[GCC 5.4.0 20160609] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import tensorflow
>>> tensorflow.__path__
['/home/yurong/.local/lib/python3.5/site-packages/tensorflow']
```
好的，根據上面的範例的路徑意思就是我tensorflow裝在我自己的路徑下。<br>
然後下面這個範例是我裝到公共路徑下<br>
```bash
>>> import jieba
>>> jieba.__path__
['/usr/local/lib/python3.5/dist-packages/jieba']
```

# python的版本問題
觀察一下以下指令<br>
```bash
$ which python3
/usr/bin/python3
$ ls -al /usr/bin/python3*
lrwxrwxrwx 1 root root       9  7月 23 01:03 /usr/bin/python3 -> python3.5
-rwxr-xr-x 1 root root 4456208  7月 22 23:19 /usr/bin/python3.5
lrwxrwxrwx 1 root root      33  4月 17 23:25 /usr/bin/python3.5-config -> x86_64-linux-gnu-python3.5-config
-rwxr-xr-x 1 root root 4456208  4月 17 23:25 /usr/bin/python3.5m
lrwxrwxrwx 1 root root      34  4月 17 23:25 /usr/bin/python3.5m-config -> x86_64-linux-gnu-python3.5m-config
-rwxr-xr-x 2 root root 4727904 12月 20  2019 /usr/bin/python3.6
-rwxr-xr-x 2 root root 4727904 12月 20  2019 /usr/bin/python3.6m
lrwxrwxrwx 1 root root      16  3月 23  2016 /usr/bin/python3-config -> python3.5-config
lrwxrwxrwx 1 root root      10 12月 11  2018 /usr/bin/python3m -> python3.5m
lrwxrwxrwx 1 root root      17  3月 23  2016 /usr/bin/python3m-config -> python3.5m-config
```
你可以發現你平常輸入的python3其實是一個link file指到python3.5，如果你想要把版本改成python3.6，可以做以下指令把link改指到3.6<br>
```bash
$ cd /usr/bin
$ unlink python3
$ ln -s python3.6 python3
$ ls -al /usr/bin/python3* 
lrwxrwxrwx 1 root root       9  7月 23 00:58 /usr/bin/python3 -> python3.6
-rwxr-xr-x 1 root root 4456208  7月 22 23:19 /usr/bin/python3.5
lrwxrwxrwx 1 root root      33  4月 17 23:25 /usr/bin/python3.5-config -> x86_64-linux-gnu-python3.5-config
-rwxr-xr-x 1 root root 4456208  4月 17 23:25 /usr/bin/python3.5m
lrwxrwxrwx 1 root root      34  4月 17 23:25 /usr/bin/python3.5m-config -> x86_64-linux-gnu-python3.5m-config
-rwxr-xr-x 2 root root 4727904 12月 20  2019 /usr/bin/python3.6
-rwxr-xr-x 2 root root 4727904 12月 20  2019 /usr/bin/python3.6m
lrwxrwxrwx 1 root root      16  3月 23  2016 /usr/bin/python3-config -> python3.5-config
lrwxrwxrwx 1 root root      10 12月 11  2018 /usr/bin/python3m -> python3.5m
lrwxrwxrwx 1 root root      17  3月 23  2016 /usr/bin/python3m-config -> python3.5m-config
```
之後你要安裝套件，就也會自動安裝到python3.6的版本，如['/home/yurong/.local/lib/python3.ˊ/site-packages/']或['/usr/local/lib/python3.6/dist-packages/']路徑底下<br>

# 使用update-alternatives做python版本管理
而管理python版本，還有另一個方法，就是使用update-alternatives，相關說明看以下網址<br>
https://www.itread01.com/content/1544468787.html<br>
我以下紀錄一下，實用的指令<br>
這是對某個特定版本的python，給予priority，數值越高，就越優先使用該版本，下面的範例就是會優先使用python3.6因為priority是3，是最高的。<br>
```bash
$ sudo update-alternatives --install /usr/bin/python python /usr/bin/python2.7 1
$ sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.5 2
$ sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.6 3
```
而以下指令是更改要使用哪個版本的python<br>
```bash
$ sudo update-alternatives --config python
There are 3 choices for the alternative python (providing /usr/bin/python).

  Selection    Path                Priority   Status
------------------------------------------------------------
  0            /usr/bin/python3.6   3         auto mode
  1            /usr/bin/python2.7   1         manual mode
* 2            /usr/bin/python3.5   2         manual mode
  3            /usr/bin/python3.6   3         manual mode

Press <enter> to keep the current choice[*], or type selection number: 0
$
$ python
Python 3.6.10 (default, Dec 19 2019, 23:04:32) 
[GCC 5.4.0 20160609] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```
如果使用update-alternatives做python版本管理的話，安裝套件用python安裝亦可，python3亦可，以下兩種指令都可以達到一樣的目的<br>
```bash
$ python -m pip install jieba --user
```
```bash
$ python3 -m pip install jieba --user
```
因為python和python3都是指向一樣的版本，下列指令可證明<br>
```bash
$ which python
/usr/bin/python
$ ls -al /usr/bin/python*
lrwxrwxrwx 1 root root      24  7月 22 23:27 /usr/bin/python -> /etc/alternatives/python
$ ls -al /etc/alternatives/python
lrwxrwxrwx 1 root root 18  7月 23 01:12 /etc/alternatives/python -> /usr/bin/python3.6
```
```bash
$ which python3
/usr/bin/python3
$ ls -al /usr/bin/python3
lrwxrwxrwx 1 root root 9  7月 23 01:23 /usr/bin/python3 -> python3.6
```


# 在jupyter notebook新增不同的python kernel
首先先安裝ipykernel
```bash
$ python3 -m pip install ipykernel
```
然後輸入以下指令，就可以在當下環境的python3加入jupyter notebook，並給定你想給的名稱"myenv"
```bash
$ python3 -m ipykernel install --name "myenv" --user
```
看看你有哪些python kernel
```bash
$ jupyter kernelspec list
```
刪除python kernel
```bash
$ jupyter kernelspec uninstall myenv
```
