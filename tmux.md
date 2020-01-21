## tmux 使用教學
### 安裝
```console
$ sudo apt install tmux
```
### 介紹
當你使用ssh遠端連線至server，然後你需要執行一個長時間運行的程式（例如訓練深度學習模型），此時儘管你使該process背景運行，一但你的terminal登出了，你執行的所有process也會跟著結束
```console
$ python3 train.py &
$ logout
```
這時後tmux的就相當方便，他可以做到在你ssh斷線後，程式繼續背景執行。
