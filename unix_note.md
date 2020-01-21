## gdb使用撇步
### reading symbols from execution file
symbol就是.c檔編譯成執行檔的時候，在組合語言裡都會有symbol標示各個function所在，以及GOT，PLT，之類的，方便開發人員在gdb使用。<br>
> 在編譯時，多下一個-g的flag，就會在binary file裡儲存symbol的資訊，例如：<br>
```bash
$ gcc -o test -Wall -g main.c
```
### 使用gdb開啟一個執行檔
```console
$ gdb test
```
### 查看source code
如果你的.c檔在當前路徑下就能查看<br>
```console
(gdb) list
```
### 設立停頓點
這裡的main是一個symbol<br>
```console
(gdb) b main
```
也可以停頓在特定行數，這是停頓在12行<br>
```console
(gdb) b 12
```
### 執行起來
如果你沒設立停頓點，就會正常執行，如果有設立停頓點，就會停在停頓點<br>
```console
(gdb) run
```
繼續執行 (continue)
```console
(gdb) c
```
### 查看frame pointer
rip是當前指令執行到哪，rbp是fucntion在stack的base，rsp是stack的頂端(pop, push的地方)
```console
(gdb) info frame
```
### 查看暫存器
```console
(gdb) info registers
```
### 查看變數值，或者呼叫函式
```console
(gdb) print i
(gdb) print get_flag()
```
### 查看assembly
離開layout畫面，就<kbd>ctrl</kbd>+<kbd>x</kbd>然後<kbd>a</kbd>
```console
(gdb) layout asm
```
***
## Memory 配置
以下都是親自透過觀察得出的結論
### stack
在main function內的變數都存在stack裡面，pointer也是，pointer會在stack內儲存該pointer指向的address<br>
### malloc
在function裡面malloc，即使跳出function外，也依舊要自己負責，如果不free掉，即使已經結束function，依舊佔空間。
### 程式在stack中的狀態


|                       |                 stack                  |
| ---------------------:|:--------------------------------------:|
|        low addr       |                                        |
|                       |                unused                  |
|          rsp ->       |     variable in function B (rbp-0xc)   |
|                       |     variable in function B (rbp-0x8)   |
|                       |     variable in function B (rbp-0x4)   |
|  rbp of function B -> |            rbp of function A           |
|                       |   ret address in A (after function B)  |
|                       |       parameter #1 of B (rbp+0x4)      |
|                       |       parameter #2 of B (rbp+0x8)      |
|                       |     variable in function A (rbp-0x8)   |
|                       |     variable in function A (rbp-0x4)   |
|  rbp of function A -> |         rbp of main function           |
|                       | ret address in main (after function A) |
|                       |       parameter #1 of A (rbp+0x4)      |
|                       |       parameter #2 of A (rbp+0x8)      |
|                       |       parameter #3 of A (rbp+0xc)      |
|                       |          variable in main (rbp-0x8)    |
|                       |          variable in main (rbp-0x4)    |
|     rbp of main ->    |     ret address after main function    |
|       high addr       |                                        |

程式進入function前，會先把參數push進stack，而且順序是從最後一個參數往第一個參數push，這樣在function內取第一個參數就rbp+0x4，第二個參數就rbp+0x8，依此類推，然後再push return address，然後push caller的rbp，然後才是依序push function會用到的local variable。
