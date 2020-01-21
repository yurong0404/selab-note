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
### 教學
輸入tmux即可創建並進入新的session
```console
$ tmux
```
然後在tmux的session底下執行程式
```console
$ python3 train.py
```
再來輸入<kbd>ctrl</kbd>+<kbd>b</kbd>然後<kbd>d</kbd>，即可離開該session，並回到原本的ssh連線的介面，之後即使你有無結束ssh連線，tmux的session都會繼續運行<br>
當你過一段時間，想查看tmux的session狀態，輸入以下指令，即可重新進入session
```console
$ tmux attach
```
tmux亦可新增多個session，輸入以下指令可檢視session列表
```console
$ tmux list-sessions
```
若你在tmux的session內想關閉你所在的session，輸入<kbd>ctrl</kbd>+<kbd>b</kbd>然後<kbd>:</kbd>然後`kill-session`，即可關閉session
### 更多資訊
https://askubuntu.com/questions/8653/how-to-keep-processes-running-after-ending-ssh-session
