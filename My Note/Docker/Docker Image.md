
## 建置 Docker Image

```
$ docker image build --tag adam .
```

	--tag 
	給予要建置的映像檔標籤名的意思
	
	. 
	後面有一個非常重要的 . ， 代表是 這裡 的意思
	表示 Docker 預設在這個目錄尋找 Dockerfile，並以其建立映像檔

### 如果需根據 staging、production 分別有好幾種不同的建置方式該怎麼做呢？

	--file 

以下以建立 production 的映像檔為例：

```
$docker image build --tag adam:production --file Dockerfile.production .
```




## 建置 Docker Image 的快取機制

 Docker 建置映像層中還有一個有趣的機制，
 若上層的映像層重新建置，則其以下的所有映像層都將重新建置，
 如果我們更動了 **ENV** 這個指令的映像層，則其以下的 **RUN**、**WORKDIR**、**EXPOSE** 等等，
 都將重新建置，也是導致建置時間大幅提升的主因


## 重新整理 Dockerfile 的執行順序

	- 指令執行的順序不會影響到容器的啟動
	- 所有的映像檔都是透過另一個映像檔作為基底
	- 擺放位置： 放在越上面的指令 變動機率越低
		- 變動機率最低的指令是 CMD 及 EXPOSE
			- FROM:
			- EXPOSE: 在設定好後就很少會進行變動
			- CMD: 啟動一個應用程式的初始指令基本上都會相同，即便更換了版本，
						  或是檔案做了什麼異動，啟動的方式都還是大同小異
			- ENV: 變動頻率得視專案而定，位置順序 看異動情況取捨
		- 
			- WORKDIR: 可能需要新的套件
			- RUN apk...: 安裝套件
			- COPY: 

## 多階段建置 Docker Image

	- $ COPY --form  從另一個映像檔複製檔案到現階段的映像檔

```js
// Dockerfile 範例 

FROM alpine:3.16.2 AS builder // # 建置階段 
RUN echo 'Builder' > /example.txt // # 建置階段 

// 建置階段：
// 以 alpine:3.16.2 這個映像檔作為基礎，並且簡單的執行了一個 RUN 的指令，
// 作用是把 Builder 這段文字寫入 example.txt 這個檔案，就結束任務了。

--------------------------------------------

FROM alpine:3.16.2 AS tester # 測試階段 
COPY --from=builder /example.txt /example.txt # 測試階段 
RUN echo 'Tester' >> /example.txt # 測試階段 

// 測試階段：
// Dockerfile 讀到了第二個 FROM，所以當作為一個新的開始
// 以 alpine:3.16.2 為基礎
// 但 這邊使用了 COPY --from=builder ....
// Docker 理解為 
// 要從 builder 這個階段複製一份 example.txt 到現在這個階段內並命名為 example.txt
// Docker 會去找 builder 這個階段，但其實我們已經把第一階段命名好了，
// 可以看到第一個 FROM 的後面 用了 AS 這個語法，將第一個階段命名為 builder

// 接著再把 Tester 這段文字寫入 example.txt 檔案中，
// 也就遇到第三個 FROM 並結束了第二個階段


--------------------------------------------

FROM alpine:3.16.2 # 最終階段 
COPY --from=tester /example.txt /example.txt 
# 最終階段CMD [ "cat", "/example.txt" ] # 最終階段

// 最終階段:
// 使用了 COPY --from=test ...
// 把 test 這個階段的 example.txt 複製過來最終階段
// 並且命名為 example.txt
// 處理的事情和 第二(測試)階段一樣
// 只是最後使用 CMD 並去讀取 example.txt 的內容


--------------------------------------------
```