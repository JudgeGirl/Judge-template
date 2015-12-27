## README ##

### judge.py ###

對於同一份 source code，利用 define 替換掉要求的 function name，各自編成 object file 再產出執行檔案。

> 不推薦這種寫法。

```
def build():
	T = '(%s 2>&1; echo $? >es) | tiger 4096; exit `cat es`'
	while True:
		if lid == 0:
			if os.system(T % 'gcc -std=c99 -O2 -c main.c time.c -lm') != 0: break
			if os.system(T % 'gcc -std=c99 -O2 -c time.c -o h12.o -lm') != 0: break
			if os.system(T % 'gcc -std=c99 -O2 -c time.c -o h24.o -DH24 -DinitTime=initTime24 -DsetTime=setTime24 -DaddTime=addTime24 -DprintTime=printTime24 -lm') != 0: break
			if os.system(T % 'gcc -std=c99 -O2 main.o h12.o h24.o -lm') != 0: break
			assert os.system('mv a.out /sandbox/sandbox/app') == 0
```