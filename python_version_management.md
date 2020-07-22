# python中套件的路徑問題
因為在實驗室每次在使用pip安裝套件時，很常就遇到Permission denied的error，然後也搞不懂到底要怎妥善的管理OS底下各個使用者所用的python，以下說明一次搞清楚！！<br>
首先根據python3執行檔的所在路徑，我們可知python3是OS底下所有使用者共用<br>
```bash
$ which python3
/usr/bin/python3
```
## python的套件安裝方法
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
安裝python套件在自己使用者的路徑下，標準作法如下<br>
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

再來，我要示範套件安裝到所以使用者都會access到的路徑，加sudo就會裝到公共路徑"/usr/local/lib/python3.5/dist-packages/"底下了，如底下指令所示，但為了不影響其他使用者，請盡量別用以下方法做安裝套件<br>
```bash
$ sudo python3 -m pip install jieba
Processing ./.cache/pip/wheels/af/e4/8e/5fdd61a6b45032936b8f9ae2044ab33e61577950ce8e0dec29/jieba-0.42.1-cp35-none-any.whl
Installing collected packages: jieba
Successfully installed jieba-0.42.1
$ ls /usr/local/lib/python3.5/dist-packages/ | grep jieba
jieba
jieba-0.42.1.dist-info
```
