## 這篇是教你一些關於sublime設定的教學

***
## Package - ctags
### Step1
在sublime，Ctrl+Shift+P -> Package Control: Install Package -> ctags，安裝一下

### Step2
到ctags的[網站](https://github.com/universal-ctags/ctags)下載ctags的執行檔

### Step3
拿到執行檔ctags.exe後，看你想把它放在哪，然後到sublime的Preferences -> Package Settings -> Ctags，把Settings - Default的東西全部copy paste到Settings - User<br>
然後在command的地方設成執行檔的路徑
![image](https://github.com/yurong0404/selab-note/blob/master/img/SublimeCtagsCscope.PNG)

### Step4
然後就可以在sublime的最頂層資料夾按右鍵-> Ctags: Rebuild Tags

***
## Package - Cscope
### Step1
在sublime，Ctrl+Shift+P -> Package Control: Install Package -> cscope，安裝一下

### Step2
到cscope的[網站](https://packagecontrol.io/packages/Cscope)下載cscope的執行檔

### Step3
拿到執行檔cscope.exe後，看你想把它放在哪，然後到sublime的Preferences -> Package Settings -> CscopeSublime，把Settings - Default的東西全部copy paste到Settings - User<br>
然後在executable的地方設成執行檔的路徑
![image](https://github.com/yurong0404/selab-note/blob/master/img/SublimeCtagsCscope_2.PNG)

### Step4
先解釋一下，cscope的邏輯是，你需要在你的專案資料夾底下有cscope.files這個檔案，這個檔案裡面放cscope需要scan過symbol的files<br>
在你帳戶的.bashrc底下加這兩行。cscope_init_file那行是列出所有程式相關的檔案，加到cscope.files<br>
cscope_database那行是要Generate the Cscope database

```console
alias cscope_init_file='find . -name '*.py' -o -name '*.txt' -o -name '*.c' -o -name '*.h' -o -name '*.cpp' -o -name '*.html' -o -name '*.css'> cscope.files'
alias cscope_database='cscope -b -q -k'
```

### Step5
到你的專案資料夾底下，依序執行cscope_init_file和cscope_database。然後預期會有一些檔案產出
![image](https://github.com/yurong0404/selab-note/blob/master/img/SublimeCtagsCscope_3.PNG)

### Step6
然後到你的sublime的最頂層資料夾按右鍵-> Cscope: Rebuild databases。然後你就能在你的code隨便一個symbol按右鍵->Cscope: Look up symbol -> Enter查詢cscope.files內出現該symbol的地方

***
## Package - HighlightWords
### Step1
在sublime，Ctrl+Shift+P -> Package Control: Install Package -> HighlightWords，安裝一下

### Step2
在code裡面使用快捷鍵Ctrl+Alt+H來highlight和反highlight symbols

***
## Theme
我用Guna和Visual Studio Code Plus Scheme

***
## reference
http://cscope.sourceforge.net/large_projects.html