## README ##

若從標準輸入讓使用者決定要讀哪一個檔案，同時也指派要寫入的檔案名稱


### 測資點設定 ###

在 `subtasks.py` 中，在每組測資最後的欄位增加額外提供的 data file (如下 `students.bin` 是讀入額外資料，`1.in` 視為標準輸入串流、`1.out` 則是正確答案，`students.html` 為該測資點預期輸出的檔案名稱)，在隨後的 `judge.py` 中將以 data file name 的方式進行移動。


```
[
 [ 10, ['1.in', '1.out', 1, 64 << 20, 64 << 10, 'students.bin', 'students.html']]
]
```

由於是額外的資料檔案，需要額外寫在 `send.lst` 中進行提攜，`send.lst` 格式如下，若有多個檔案，一行一個檔案名稱。


```
students.bin
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

### 測資比對 ###


抓取每一個測資點的預期施出檔案名稱，接著與正確輸出檔案比較。


```
...
		for trial in subtask[1:]:
			ifn, ofn, tl, ml, ol, dfn, odfn = trial
			get(ifn)
...
				assert log[0] == 'OK'
				get(ofn)
				if os.system('./special \'%s\' \'%s\' /sandbox/sandbox/ws/%s >/dev/null 2>&1' % (ifn, ofn, odfn)) != 0:
					tvd = const.WA
				else:
					tvd = const.AC
```