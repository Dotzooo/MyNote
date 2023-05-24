
	- Dockerfile: 執行容器時的說明書，也是構築映像檔的步驟

## FROM 

 每一個映像檔都必須以其他的映像檔作為基底
	

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

### 為什麼 RUN 這個指令後面會有 && ( 反斜線 ) 這個符號呢？

## WORKDIR

## COPY

## RUN

## EXPOSE

## CMD