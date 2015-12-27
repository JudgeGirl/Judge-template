## README ##

每一個 testcase 都運行不同的 `#define ` 參數，用不同編譯出的執行檔測試。


### subtasks.py ###

```
[
 [ 50, ['1.in', '1.out', 1, 64 << 20, 64 << 20, '-DHUNDRED']]
,[ 50, ['6.in', '6.out', 1, 64 << 20, 64 << 20, '-DAPLUS']]
]
```

### judge.py ###

```
def run(tl, ml, ol, ifn, DFLAG):
	T = '(%s 2>&1; echo $? >es) | tiger 4096; exit `cat es`'
	if os.system(T % ('gcc -std=c99 -O2 a.c -lm %s' % DFLAG)) != 0:
		exit(0, const.CE, 0, 0, read('/run/shm/slave.out'))
	assert os.system('mv a.out /sandbox/sandbox/app') == 0
```

對於不同的 `gcc -DFLAG` 參數進行編譯，同時導出編譯成功訊息。同樣地，完全都沒有 `-DFLAG` 的情況也進行編譯測試。


```
def build():
	T = '(%s 2>&1; echo $? >es) | tiger 4096; exit `cat es`'
	while True:
		if lid == 1:
			os.rename('source', 'a.c')
			if os.system(T % 'gcc -std=c99 -O2 a.c -lm') != 0: break
			assert os.system('mv a.out /sandbox/sandbox/app') == 0
```