# Setting linux
### 로그 아웃
```bash
# exit
# logout
```
---
### 시스템 종료
```bash
[root@localhost ~]# shutdown –h now              ( 지금  halt  )

[root@localhost ~]# shutdown –h +10                  ( 10분 후에 halt  )
[root@localhost ~]# shutdown –r   22:10                      ( 22:10 에 reboot  )
[root@localhost ~]# shutdown –c                                   ( 진행 중인 shutdown cancel  )
[root@localhost ~]# shutdown –h +10  "hurry up"      ( 지금 접속 중인 사용자에게 메세지 전송 하고 10분 후에  halt )
```
```bash
[root@localhost ~]# halt
shutdown -h now 와 동일
```
```bash
init
런레벨 (시스템 실행 모드, 0~6)를 설정하는 명령으로 0(종료)을 주게 되면 종료된다.
[root@localhost ~]# init 0

[root@localhost ~]# systemctl poweroff       (Halt &  Power-Off)
[root@localhost ~]# systemctl halt      (Halt)
```
---
### 시스템 재시작

```bash
# reboot

(또는)

# init 6

(또는)

# shutdown –r now

# systemctl  reboot

```
---
### Run level
```bash
#systemctl rescue (런레벨1)

#systemctl isolate multi-user.target (런레벨2)
#systemctl isolate runlevel3.target (런레벨2)

#systemctl isolate graphical.target (런레벨5)
#systemctl isolate runlevel5.target (런레벨5)
```

- 리눅스는 시스템이 기동될 때 런레벨(Run Level: 실행레벨) 값을 참조한다.
- 다음 런레벨은 리눅스 서버 관리자가 알아야 할 값들이다.
```
0 :  시스템 정지
1 :  싱글 유저 모드로 기동 (시스템 복구시 사용)
2 :  멀티 유저 (네트워크를 사용하지 않는 텍스트 멀티 유저모드)
3 :  멀티 유저 (일반적인 쉘 기반의 텍스트 멀티 유저 모드)
4 :  사용하지 않음
5 :  GUI 멀티 유저 모드이다.(x-window)
6 :  시스템 재기동  
```
- 일반적으로 서버를 운용할 때는 3 또는 5를 지정한다.
- 보통 서버에서는 GUI가 필요가 없기 때문에 3를 지정하는 것이 보통이다.
- RHEL7 에서는 init이 systemd 로 대체되어 숫자 기반의 런레벨이 아니라 각 런레벨에 대한 설정 세트를 통해서 런레벨을 변경 한다. 
---
### 패키지 설치 및 업데이트
```bash
[root@localhost ~]# yum repolist
[root@localhost ~]# yum update -y
```
- yum 커맨드를 사용해서 패키지 업데이트 및 새로운 패키지 설치가 가능하다.
- 인스톨 CD의 패키지가 업데이트 되었을 수 있기 때문에 업데이트하는 것이 좋다.
- 확인 메시지에 y를 입력하면 업데이트를 진행 한다. 
- 커널 업데이트를 포함 하는 경우 재부팅 한다.
---
### 새 패키지 설치 (Install)
```bash
[root@localhost ~]# yum install cronie

[root@localhost ~]# yum install rdate

[root@localhost ~]# yum install gcc

[root@localhost ~]# yum install make

[root@localhost ~]# yum install wget

[root@localhost ~]# yum install gcc-c++

[root@localhost ~]# yum install cmake

[root@localhost ~]# yum install net-tools

[root@localhost ~]# yum install bind-utils

[root@localhost ~]# yum install psmisc
```
---
### 그 밖에 자주 사용되는 yum 커맨드
```bash
# yum clean all                  패키지 캐시 파일 삭제
# yum list                          모든 패키지 표시
# yum remove                      <패키지 명>  패키지 삭제
# yum update                    <패키지 명>  패키지 업데이트
```
---
### 서버 시간 동기화
- 서버를 운영하다 보면 시간 동기화를 하지 않을 경우 시간이 조금씩 어긋나게 된다. 이를 막아주기 위해서 rdate 혹은 다른 방법을 이용해서 서버시간을 국제 시간에 동기화 시켜 주어야 한다.
```bash
# cd /etc/cron.hourly/

# vi time_sync.sh

# chmod 700 time_sync.sh
```
- 실행 스크립트 time_sync.sh를 /etc/cron.hourly 밑에 생성한다. 
- 실행하기 위해 파일 권한을 700을 준다.
### 실행 스크립트
```bash
#!/bin/bash
rdate -s time.bora.net && date && clock -r && clock -w > /dev/null 2>&1
```
### cron 작업
- 시스템 운영에 필요한 일상적이고 주기적인 작업을 지정된 시간에 반복적으로 수행하기 위한 목적으로 cron이
   제공된다. 
- cron은 crond라는 데몬으로 작동하며 리눅스를 설치할 때에도 기본적으로 작동하도록 설정되어 있다. 
- cron은 시스템 관리를 위해 꼭 필요한 작업이며, 반드시 구동해야 할 프로그램이다.
- cron은 /etc/crontab 이라는 설정 파일과  /etc 밑에 cron.hourly, cron.daily, cron.weekly, cron.monthly로 구성되며, 
   실행 시간, 실행 권한, 명령어 등을 적는것만으로 간단하게 설정할 수 있다.
---
### 사용자 계정 추가
- root계정은 시스템 설정 변경, 패키지 설치 등 필요할 때만 사용하는 계정이고 로그인(특히, 원격 로그인)등에 사용할  
   계정으로 보안상 위험한 계정이다. 
- root 계정은 시스템에서 어떤 일도 할 수 있는 계정이기 때문에 작업 중 사고가 발생할 가능성이 많다.
- 가급적 root 계정은 꼭 필요할 때만 사용하고 바로 일반 계정으로 돌아와 작업을 해야 한다.
- 따라서, 로그인 계정이 필요하며 계정은 사용자 계정 추가 명령으로 추가할 수 있다.
```bash
[root@localhost ~]# useradd -g wheel -d /home/kickscar kickscar
[root@localhost ~]# passwd kickscar
kickscar 사용자의 비밀 번호 변경 중
새  암호:
새  암호 재입력:
passwd: 모든 인증 토큰이 성공적으로 업데이트 되었습니다.
[root@localhost ~]# 
```
 1. useradd 또는 adduser 명령어를 사용한다.
 2. -g 뒤에 그룹 명을 준다. –d 뒤에는 계정 홈 디렉터리이다.
 3. 로그인을 하기 위해서는 passwd 명령어를 사용해서 비밀번호를 설정해 주어야 한다.
---
### 로그인 스크립트
- 로그인을 하게 되면 몇 가지 설정 스크립트가 실행되고 계정의 환경을 설정하게 된다.
- 순서는  /etc/profile -> ~/.bash_profile -> ~/.bashrc -> /etc/bashrc
### auto logout
- 해당 시간동안 입력이 없으면 자동으로 로그아웃 시키는 스크립트 추가
```bash
[kickscar@localhost ~]$ su -
암호:
[root@localhost ~]# vi /etc/profile
```
>  export TMOUT = 300
>> 다음 내용 추가







