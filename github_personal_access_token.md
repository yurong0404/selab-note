## 這是一篇教你怎麼解決push commit時遇到以下error的教學
> 如下
![image](https://github.com/yurong0404/selab-note/blob/master/img/PersonalAccessToken.PNG)

### Step1:
> ![image](https://github.com/yurong0404/selab-note/blob/master/img/PersonalAccessToken_2.PNG)

### Step2:
> ![image](https://github.com/yurong0404/selab-note/blob/master/img/PersonalAccessToken_3.PNG)

### Step3:
> 到你的終端機設定一下，<your_token>是你Step2設定完可以得到的token，<USERNAME>是你的帳號名稱，<REPO>就你的repository
```console
git remote set-url origin https://<your_token>@github.com/<USERNAME>/<REPO>.git
```

### Step4:
> push commit
> ![image](https://github.com/yurong0404/selab-note/blob/master/img/PersonalAccessToken_4.PNG)