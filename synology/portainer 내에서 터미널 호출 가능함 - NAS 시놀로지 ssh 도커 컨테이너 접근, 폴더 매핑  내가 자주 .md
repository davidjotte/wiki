





ssh putty를 통한 접속과 기본적인 명령어  
도커 컨테이너 볼륨과 NAS 폴더 매핑 기능에 대해 다룬다.

___

### **  
CMD**

```
ssh synology-id@domain -p port
```

**도메인이 없다면 ip**  
**도메인**: test.synology.me  
**아이디**: id123  
**포트**: 22

```
ssh id123@test.synology.me -p 22
```

### **PUTTY**

![](https://blog.kakaocdn.net/dn/bwxQUn/btsKbjhdscD/DU6xs7Z2lMZUjRBkm0UDn0/img.png)

putty 접속

![](https://blog.kakaocdn.net/dn/wsD7l/btsKb1fDG8C/gsxBNPwPaqV2okgeC6H3cK/img.png)

로그인

### **진입**

![](https://blog.kakaocdn.net/dn/b7USeG/btsKb6A7C56/kR9fJdnddwFCGYkxJshXy0/img.png)

로그인 후 sudo -i를 통해 관리자 권한을 가져온다

시놀로지는 보통 기본적으로 root 계정 자체는 비활성화시켜 놓기 때문에  
일반 사용자로 로그인 후 관리자 권한을 가져오는 것.

root로 변경되면 완료.

### **DOCKER**

이제 도커에 접근하려면

```
docker <span>exec</span> -it [container_name] /bin/sh
```

**docker**: 도커 호출  
**exec**: 현재 실행 중인 도커에 다음 명령을 실행  
**\-it:** -i와 -t를 같이 쓴 명령어, 컨테이너가 우리와 상호작용 할 수 있게 함 ( 쉘을 통한 명령어 입력, 결과 출력 등 )  
**\[container\_name\]**: 현재 실행 중이며 명령어 입력할 컨테이너 이름  
**/bin/sh**: 컨테이너 내 쉘 실행

![](https://blog.kakaocdn.net/dn/eKHVhX/btsKaLE8Wal/ByAh0aTxpDcDXDlHniam00/img.png)

나는 보통 컨테이너에서 뭔가 작업할 땐 기본적으로 이렇게 까지는 고정이다.

### **매핑**

컨테이너 내부 디렉터리는 NAS DMS에선 접근이 안되기 때문에 폴더 매핑을 지원한다.

![](https://blog.kakaocdn.net/dn/csUxXG/btsKbhcuRyM/QcFQuo3r5ZSC1BzF9t53XK/img.png)

현재 이 매핑은 NAS volume에 있는 /docker/n8n 폴더와  
컨테이너 내부에 있는 /n8n이란 폴더를 매핑한다.

![](https://blog.kakaocdn.net/dn/FQ1vN/btsKcTgZOTB/vs2Y6OkmPUy3cz5YB6hmBk/img.png)

/docker/n8n 위치에 mount.txt 파일을 만들었다.  
현재 이 파일은 Synology File Station으로 확인 가능하다.

만약 제대로 매핑이 되었다면,  
컨테이너 내부 /n8n 폴더에도 이 파일이 있어야 한다.

다시 PUTTY로 돌아가서

컨테이너 진입 후,  
cd / 를 통해 컨테이너 최상위로 이동한다

```
<span>cd</span> /
```

ls 명령어로 모든 콘텐츠를 확인한다.

```
ls
```

![](https://blog.kakaocdn.net/dn/mQrSr/btsKayMUNq4/gdj5p2Q8sV8Ub1E5fheQKk/img.png)

n8n이 보인다.

![](https://blog.kakaocdn.net/dn/cqKe7G/btsKcjmKgf6/XArcSpjTPcBRTJgfGFlAHK/img.png)

n8n으로 이동 후 ls  
매핑된 파일 mount.txt가 보인다.

___

폴더를 매핑시켜 컨테이너가 자동으로 수집한 데이터나  
로그 파일에 쉽게 접근할 수 있는 등.. 알아두면 좋을 것 같다.