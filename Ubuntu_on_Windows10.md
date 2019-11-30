這篇是在教你如何在Windows 10上裝一個Ubuntu的系統

## Windows 10設定
### Step1:
首先你必須到Windows 10的設定->應用程式->應用程式與功能->相關設定->程式與功能<br>
![image](https://github.com/yurong0404/selab-note/blob/master/img/WindowsSetting.PNG)
<br><br>
### Step2:
然後會跳出一個視窗，請點選"開啟和關閉Windows功能"，並將"使用於Linux的Windows子系統"核取方塊勾選起來，並按下確定。然後系統應該會跳出需要重新開機。<br>
![image](https://github.com/yurong0404/selab-note/blob/master/img/WindowsSetting2.PNG)
<br>重新開機過後請繼續執行下面的步驟。<br>

---
## 在Windows Store安裝Ubuntu
### Step1:
先至Windows搜尋欄搜尋開啟Microsoft Store<br><br>
![image](https://github.com/yurong0404/selab-note/blob/master/img/SearchMicrosoftStore.PNG)

### Step2:
搜尋Ubuntu並且安裝它<br><br>
![image](https://github.com/yurong0404/selab-note/blob/master/img/SearchUbuntu.PNG)

### Step3:
安裝完它後，在Windows搜尋Ubuntu並執行它，此時它會開始在你的Windows安裝一個Ubuntu的子系統，版本應該是Ubuntu當前最新版本，安裝過程你會需要輸入帳戶名稱和密碼。<br>
![image](https://github.com/yurong0404/selab-note/blob/master/img/SearchUbuntu2.PNG)
<br><br>安裝<br>
![image](https://github.com/yurong0404/selab-note/blob/master/img/InstallUbuntu.PNG)
<br><br>安裝完成後你可以在Windows search執行Ubuntu或者bash，就可以執行Ubuntu的bash了，正常使用Linux指令也沒問題。你也可以選擇在cmd.exe輸入bash亦可以進入Ubuntu的bash。<br><br>
![image](https://github.com/yurong0404/selab-note/blob/master/img/cmdexe.PNG)
<br>

---
## 安裝Hyper
再來，由於我看不慣Windows內建的命令提示字元介面，所以我這邊建議大家安裝一個軟體叫做Hyper，它提供比較漂亮的介面，和一些小插件。<br>

### Step1:
到它的官網下載Windows版本，並安裝。<br>
https://hyper.is/<br>
![image](https://github.com/yurong0404/selab-note/blob/master/img/HyperHomepage.PNG)

### Step2:
安裝完後桌面應該會出現Hyper的捷徑，執行之後，它會預設執行Windows內建的操作介面。<br>
![image](https://github.com/yurong0404/selab-note/blob/master/img/HyperDefaultcmd.PNG)

### Step3:
這時我們要把預設操作介面改成Ubuntu的bash，請到Hyper介面左上角的三條槓->Edit->Preferences，並將shell的屬性修改成以下的路徑，並儲存。<br>
```console
    // the shell to run when spawning a new session (i.e. /usr/local/bin/fish)
    // if left empty, your system's login shell will be used by default
    //
    // Windows
    // - Make sure to use a full path if the binary name doesn't work
    // - Remove `--login` in shellArgs
    //
    // Bash on Windows
    // - Example: `C:\\Windows\\System32\\bash.exe`
    //
    // PowerShell on Windows
    // - Example: `C:\\WINDOWS\\System32\\WindowsPowerShell\\v1.0\\powershell.exe`
    shell: 'C:\\Windows\\System32\\bash.exe',
```

### Step4:
重開Hyper之後應該就能發現它自動進入Ubuntu的bash shell了。<br>
![image](https://github.com/yurong0404/selab-note/blob/master/img/HyperDefaultDirectory.PNG)

---
## 修改bash的預設路徑
注意:在bash裡Windows的本機路徑在/mnt底下，要注意別刪到之類的<br>

### Step1:
很簡單，你的Ubuntu帳戶會安裝在/home/[帳戶名稱] 底下，請用vim編輯以下檔案，帳戶名稱記得換成自己的<br>
```console
> vim /home/yurong/.bashrc
```
把檔案的最後新增一行指令，cd /home/yurong，如此在開啟bash的時候都會先把路徑改到home目錄<br>
```console
# enable programmable completion features (you don't need to enable
# this, if it's already enabled in /etc/bash.bashrc and /etc/profile
# sources /etc/bash.bashrc).
if ! shopt -oq posix; then
  if [ -f /usr/share/bash-completion/bash_completion ]; then
    . /usr/share/bash-completion/bash_completion
  elif [ -f /etc/bash_completion ]; then
    . /etc/bash_completion
  fi
fi

cd /home/yurong
```
