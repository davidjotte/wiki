**ssh port 41522**

```
ssh [ggor@192.168.5.102](mailto:ggor@192.168.5.102) -p 41522  

sudo -i     ; 로 root 계정 들어가야 docker 명령 사용 가능함.  
```



### bookstack 최종 된 dock--.yml 



port 6875:80 이네..  

 https:// 아니고..   

```
services:
  db:
    image: lscr.io/linuxserver/mariadb:latest
    container_name: BookStack-DB
    hostname: bookstack-db
    mem_limit: 1g
    cpu_shares: 768
    security_opt:
      - no-new-privileges:false
    volumes:
      - /volume1/docker/bookstack/uploads:/var/www/bookstack/public/uploads:rw
      - /volume1/docker/bookstack/storage-uploads:/var/www/bookstack/storage/uploads:rw
    environment:
      TZ: Asia/Seoul
      MYSQL_ROOT_PASSWORD: rootpass
      MYSQL_USER: bookstackuser
      MYSQL_PASSWORD: bookstackpass
      MYSQL_DATABASE: bookstack
    restart: on-failure:5

  bookstack:
    image: lscr.io/linuxserver/bookstack:latest
    container_name: BookStack
    hostname: bookstack
    mem_limit: 1g
    cpu_shares: 768
    security_opt:
      - no-new-privileges:true
    ports:
      - 6875:80
    volumes:
      - /volume1/docker/bookstack/uploads:/var/www/bookstack/public/uploads:rw
      - /volume1/docker/bookstack/storage-uploads:/var/www/bookstack/storage/uploads:rw
    environment:
      DB_HOST: bookstack-db
      DB_PORT: 3306
      DB_DATABASE: bookstack
      DB_USERNAME: bookstackuser
      DB_PASSWORD: bookstackpass
      APP_KEY: base64:Y3VzMzI5YzJwMmV0OWY4cW9sNTdweDczcTkyMnE4cXM=
      APP_URL: http://bookstack.ggor.synology.me
    restart: on-failure:5
    depends_on:
      db:
        condition: service_started
```











### yaml 만들었는데 뭐가 틀린 건지.. 

```
 vi docker-compose.yml
 
 docker-compose up -d
```

헤놀로지에 ssh로 접속 후 `sudo su` 로 root권한을 획득합니다.



#### docker  설치 로 가려면 어디로 가야 하나.. 

[시놀로지 SSH 터미널에서 후 도커 컨테이너 접속하기 - NAS - 덕도](https://dukdo.com/nas/6683)

```
sudo chmod 666 /var/run/docker.sock
docker exec -t -i [컨테이너이름] /bin/bash
```

위의 두줄을 실행해주시면 됩니다. 컨테이너이름에서 []는 빼주세요. 컨테이너가 dukdo라면 docker exec -t -i dukdo /bin/bash



### Portainer - synology 설치 

```
mkdir /volume1/docker/portainer
sudo docker run -d -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /volume1/docker/portainer:/data portainer/portainer-ce
```

위 두줄만 입력해도 포테이너 설치가 완료 됩니다. 그 뒤에 접속은 본인의 DDNS 주소나 내부 IP 주소 뒤에 포트 9000을 써주시면 됩니다.

예) **[http://192.168.1.xxx:9000](http://192.168.1.xxx:9000/)**



osx 에서 수정하자면 .. 

```
mkdir /volume1/docker/portainer

sudo docker run -d -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /docker/portainer:/data portainer/portainer-ce
```



```
docker run -d --name=portainer \
-p 8000:8000 \
-p 9000:9000 \
-v /var/run/docker.sock:/var/run/docker.sock \
-v /volume1/docker/portainer:data \
--restart=always \
portainer/portaienr -ce

```

