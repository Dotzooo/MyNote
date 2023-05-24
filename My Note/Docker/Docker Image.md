
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