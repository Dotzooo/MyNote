
	- Dockerfile: 執行容器時的說明書，也是構築映像檔的步驟

## 編寫規則

	- 每條保留字 指令都比須為大寫字母且後面 至少需一個參數
	- 指令順序執行，遵循從上至下原則
	- # 表示註釋
	- 每條指令都會創建一個新的鏡像層，並對鏡像層進行提交

## 保留字

	-FROM : 指定基礎鏡像，每一個映像檔都必須以其他的映像檔作為基底


 
	

## ENV

設定運行成容器後的環境變數，以 key = value 的方式設定
運行成容器後，作業系統中就存在 $AUTHOR 的環境變數

```
docker container run --interactive --tty nginx bash
root@338a952455b4:/# echo $NGINX_VERSION
1.23.4
root@338a952455b4:/#
```


## RUN

終端機所執行的指令，以下舉例
```
RUN apk add --update --no-cache \ 
	build-base \ 
	curl
...

RUN gem install bundler:2.3.19 && \ 
	bundle install -j4 --retry 3 && \ 
	bundle clean --force && \ 
	find /usr/local/bundle -type f -name '*.c' -delete && \ 
	find /usr/local/bundle -type f -name '*.o' -delete && \ 
	rm -rf /usr/local/bundle/cache/*.gem
```

### 為什麼 RUN 這個指令後面會有 ＼ ( 反斜線 ) 這個符號呢？

關於映像檔是由映像層一層一層堆疊出來的，
每一個起始的指令都代表了一個新的映像層，**FROM** 是一層，**RUN** 也是一層，
如果安裝 `build-base` 以及 `curl` 如下面所示分開執行的話
```
RUN apk add --update --no-cache build-base 
RUN apk add --update --no-cache curl
```

這樣造成映像檔的映像層數變多，且造成不必要的資源浪費

### 為什麼 RUN 這個指令後面會有 &&  這個符號呢？

若前面的指令執行結果沒有出錯，則接著執行後面的指令

### ＼ ( 反斜線 ) 和 && 符號的意義

把指令濃縮到一個映像層之中，進而降低映像檔的映像層數。


## WORKDIR

建立一個工作目錄，並且以這個工作目錄作為預設的工作目錄


## COPY

從本機的檔案系統中複製想要的資料到容器內的檔案系統(重要的是檔案的內容，而不是檔案的名稱)
```
COPY example.txt happy.txt
# 左邊為本機的檔案名稱； 右邊為運行成容器後的檔案名稱
```



## EXPOSE

運行成容器後預設打開的 port 


## CMD

映像檔運行成為容器時所執行的第一個指令，也關係到容器是否進入停止狀態的指令