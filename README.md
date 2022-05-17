## 透過container查詢docker-compose.yml位置
```sh
docker inspect nginx18 | grep com.docker.compose
```

## install composer
__在nginx-phpfpm環境，一般放在phpfpm的container__  

_docker-compose.yml_
```docker-compose.yml
app:
    build: ./app # 指定/app資料夾底下的Dockerfile為image
    volumes:
        - ./app:/var/www/html
    ...
```

_app/Dockerfile_
```Dockerfile
FROM prestashop/prestashop:1.7-7.3-fpm

COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer
```

如此便可以下
```sh
docker exec [container_name] composer
```
值得一提的是這個composer實際是在phpfpm的container執行的  
直接進到container裡面cd到要進行composer的資料夾可能比較方便


## 修改docker-compose.yml
可以直接下`docker-compose up -d`，會自動偵測更改的地方


## internal localhost
要呼叫localhost上的其他服務，但localhost呼叫無法成功
可以使用`host.docker.internal`，但依據官網，這應該只在Mac的Docker Desktop有效
> The host has a changing IP address (or none if you have no network access). We recommend that you connect to the special DNS name host.docker.internal which resolves to the internal IP address used by the host. This is for development purpose and will not work in a production environment outside of Docker Desktop for Mac.
