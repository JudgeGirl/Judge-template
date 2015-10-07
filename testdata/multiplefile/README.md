## multiple file ##

假設要編譯多個檔案，例如使用者上傳 `max.c`, `max.h`，預設檔案 `main.c`，要將三份程式碼同時編譯。

### send.lst ###

放入非使用者的相關編譯檔案，會把這些同時送到 judge 的同一份資料夾下。撰寫時，以行為單位輸入檔案名稱。

```
main.c
```

### source.lst ###

預計要使用者上傳的兩個檔案，注意這會影響前端頁面顯示。撰寫時，以行為單位輸入檔案名稱。

```
max.c
max.h
```

### judge ###

```
...
def build():
	T = '(%s 2>&1; echo $? >es) | tiger 4096; exit `cat es`'
	while True:
		if lid == 0:
			if os.system(T % 'gcc -std=c99 -O2 max.c main.c -lm') != 0: break
			assert os.system('mv a.out /sandbox/sandbox/app') == 0
		else:
			exit(0, const.CE, 0, 0, b'unsupported language\n')
		return
	exit(0, const.CE, 0, 0, read('/run/shm/slave.out'))
...
```

請針對每一題調整編譯參數，`gcc -std=c99 -O2 max.c main.c -lm`，並且 database 連上去後，預設 language = 0 時，才會進行 multiple file compile 的操作，否則請更動 `if lid == 0` 這一行，預設 

* `lid = 1` ANSI C
* `lid = 2` C++
* `lid = 3` C#
* `lid = 4` python3
* `lid = 5` scala

### subtask.py ###

與 strict 的方式相同。

