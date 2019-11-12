## gdb使用撇步
### reading symbols from execution file
> symbol就是.c檔編譯成執行檔的時候，在組合語言裡都會有symbol標示各個function所在，以及GOT，PLT，之類的，方便開發人員在gdb使用。<br>
> 在編譯時，多下一個-g的flag，就會在binary file裡儲存symbol的資訊，例如：<br>
```bash
$ gcc -o test -Wall -g main.c
```
### 使用gdb開啟一個執行檔
```console
$ gdb test
```
### 查看source code
> 如果你的.c檔在當前路徑下就能查看<br>
```console
(gdb) list
```
### 設立停頓點
> 這裡的main是一個symbol<br>
```console
(gdb) b main
```
> 也可以停頓在特定行數，這是停頓在12行<br>
```console
(gdb) b 12
```
### 執行起來
> 如果你沒設立停頓點，就會正常執行，如果有設立停頓點，就會停在停頓點<br>
```console
(gdb) run
```
> 繼續執行 (continue)
```console
(gdb) c
```
### 查看frame pointer
> rip好像是當前指令執行到哪，rbp好像是stack的base，rsp應該是stack的頂端(pop, push的地方)
```console
(gdb) info frame
```
### 查看暫存器
```console
(gdb) info registers
```
### 查看變數值，或者呼叫函式
> 打CTF好像蠻好用的
```console
(gdb) print i
(gdb) print get_flag()
```
### 查看assembly
> 離開layout畫面，就ctrl+x a
```console
(gdb) layout asm
```

## Memory 配置
> 以下都是親自透過觀察得出的結論
### stack
> 在main function內的變數都存在stack裡面，pointer也是，pointer會在stack內儲存該pointer指向的address<br>
### malloc
> 在function裡面malloc，即使跳出function外，也依舊要自己負責，如果不free掉，即使已經結束function，依舊佔空間。
