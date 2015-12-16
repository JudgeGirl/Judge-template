## 指定檔案名稱 ##

若從標準輸入讓使用者決定要讀哪一個檔案，並且利用標準輸出進行驗正。


### 測資點設定 ###

在 `subtasks.py` 中，在每組測資最後的欄位增加額外提供的 data file (如下 `0.dat` 是額外資料，`0.in` 視為標準輸入串流、`1.out` 則是比對標準輸出串流結果)，在隨後的 `judge.py` 中將以 data file name 的方式進行移動。


```
[
 [  0, ['0.in', '0.out', 1, 64 << 20, 64 << 10, '0.dat']]
]

```

由於是額外的資料檔案，需要額外寫在 `send.lst` 中進行提攜，`send.lst` 格式如下，若有多個檔案，一行一個檔案名稱。

```
0.dat
```

### 檔案名稱設定 ###

打開 `judge.py` 修改 `def run(tl, ml, ol, ifn):` 變成 `def run(tl, ml, ol, ifn, dfn):`，也就是多一個額外的 data file name `dfn`，接著在最後一行增加

```
	assert os.system('cp -f %s /sandbox/sandbox/ws/%s' % (dfn, dfn)) == 0
```

將傳輸過去的檔案，移動到 `/sandbox/sandbox/ws/` 目錄下。

在 File I/O 處理過程中，在不同 submission 寫入檔案結果都會保留在 `/sandbox/sandbox/ws/*` (編譯出的執行檔也放置於此)，防止 submission 彼此之間干擾，額外在 `def build():` 編譯前先清空其他的檔案。

```
def build():
	assert os.system('rm -rf /sandbox/sandbox/ws/*') == 0
...
```

