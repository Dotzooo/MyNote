
	- Dockerfile: 執行容器時的說明書，也是構築映像檔的步驟

## FROM 

 每一個映像檔都必須以其他的映像檔作為基底
	

## ENV

 設定運行成容器後的環境變數

```
docker container run --interactive --tty nginx bash
root@338a952455b4:/# echo $NGINX_VERSION
1.23.4
root@338a952455b4:/#
```

## RUN

## WORKDIR

## COPY

## RUN

## EXPOSE

## CMD