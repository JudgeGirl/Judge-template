## 固定檔案名稱 ##

假設讀取固定檔案名稱和寫入固定檔案名稱

### 比對 ###

若以 binary file 進行比較，直接修改 `special.py`

```
fa, fb = open(sys.argv[2], "rb"), open(sys.argv[3], "rb")
```

同時若拿使用者寫入檔案進行比較時 (相較於標準輸出串流)，請修改 `judge.py`

```
				assert log[0] == 'OK'
				get(ofn)
				if os.system('./special \'%s\' \'%s\' /sandbox/sandbox/ws/test.enc >/dev/null 2>&1' % (ifn, ofn)) != 0:
					tvd = const.WA
				else:
					tvd = const.AC
```

若固定檔案名稱為 `test.enc`，直接寫 `/sandbox/sandbox/ws/test.enc` 即可。

### 測資點設定 ###

在 `subtasks.py` 中，在每組測資最後的欄位增加額外提供的 data file (如下 `1.in` 是額外資料，`1.key` 視為標準輸入串流、`1.out` 則是比對標準輸出串流結果)，在隨後的 `judge.py` 中將以 data file name 的方式進行移動。


```
[
 [ 10, ['1.key', '1.out', 1, 64 << 20, 64 << 10, '1.in']]
]

```

由於是額外的資料檔案，需要額外寫在 `send.lst` 中進行提攜，`send.lst` 格式如下，若有多個檔案，一行一個檔案名稱。

```
1.in
```

### 檔案名稱設定 ###

打開 `judge.py` 修改 `def run(tl, ml, ol, ifn):` 變成 `def run(tl, ml, ol, ifn, dfn):`，也就是多一個額外的 data file name `dfn`，接著在最後一行增加

```
	assert os.system('cp -f %s /sandbox/sandbox/ws/test' % (dfn)) == 0
```

將指定的檔案命名為 `test`。

在 File I/O 處理過程中，在不同 submission 寫入檔案結果都會保留在 `/sandbox/sandbox/ws/*` (編譯出的執行檔也放置於此)，防止 submission 彼此之間干擾，額外在 `def build():` 編譯前先清空其他的檔案。

```
def build():
	assert os.system('rm -rf /sandbox/sandbox/ws/*') == 0
...
```

