우분투 22.04에서 `schroot`를 사용하여 `woody` 사용자의 `/home/woody` 디렉토리를 `chroot` 환경의 루트로 설정하고, 그 안에 `/home/woody/home/woody`를 사용자의 홈 폴더로 설정하는 방법을 설명.

### 1. `woody` 사용자 생성
우선, `woody` 사용자를 생성합니다. 생성되지 않았다면 아래 명령어로 생성합니다.

```bash
sudo adduser woody
```

### 2. `schroot` 및 `debootstrap` 패키지 설치
`schroot` 및 `debootstrap` 패키지를 설치합니다.

```bash
sudo apt-get update
sudo apt-get install schroot debootstrap
```

### 3. `schroot` 설정 파일 편집
`/etc/schroot/schroot.conf` 파일을 편집하여 `woody` 사용자의 `chroot` 환경을 설정합니다.

```bash
sudo nano /etc/schroot/schroot.conf
```

파일의 끝에 다음 내용을 추가.

```plaintext
[woody]
description=Chroot environment for woody
directory=/home/woody
users=woody
root-users=woody
type=directory
```

### 4. `chroot` 환경 설치
`chroot` 환경을 설치할 디렉토리를 만들고 `debootstrap`을 사용하여 우분투 기본 시스템을 설치.

```bash
sudo mkdir -p /home/woody
sudo debootstrap --variant=minbase jammy /home/woody http://archive.ubuntu.com/ubuntu/
```

여기서 `jammy`는 Ubuntu 22.04 LTS의 코드명.

### 5. 필수 디렉토리 및 파일 설정
`chroot` 환경 내에 필요한 파일 시스템 구조를 설정하고 필요한 파일을 복사합니다.

```bash
sudo mkdir -p /home/woody/{bin,lib,lib64,usr,usr/bin,dev,proc,sys,run,home}
sudo cp -v /bin/bash /home/woody/bin/
sudo cp -v /bin/ls /home/woody/bin/
sudo cp -v /bin/mkdir /home/woody/bin/

# 필요한 라이브러리 파일 복사 (ldd 명령어를 사용하여 확인)
sudo cp -v /lib/x86_64-linux-gnu/{libtinfo.so.6,libdl.so.2,libc.so.6,libselinux.so.1} /home/woody/lib/
sudo cp -v /lib64/ld-linux-x86-64.so.2 /home/woody/lib64/ #동일 한 파일일 이라 뜨면 패스 
```

### 6. 장치 파일 생성
`chroot` 환경 내에서 필요한 장치 파일을 생성합니다.

```bash
sudo mknod -m 666 /home/woody/dev/null c 1 3
sudo mknod -m 666 /home/woody/dev/tty c 5 0
sudo mknod -m 666 /home/woody/dev/zero c 1 5
#입력 해보고 동일한 파일 이라 뜨면 패스

sudo mount --bind /dev /home/haru2/dev
# /dev 디렉토리를 /home/haru2/dev에 바인드 마운트합니다.
# 목적: /dev의 장치 파일을 /home/haru2/dev에서도 접근 가능하게 함.

sudo mount --bind /proc /home/haru2/proc
# /proc 디렉토리를 /home/haru2/proc에 바인드 마운트합니다.
# 목적: /proc의 가상 파일 시스템을 /home/haru2/proc에서도 접근 가능하게 함.

sudo mount --bind /sys /home/haru2/sys
# /sys 디렉토리를 /home/haru2/sys에 바인드 마운트합니다.
# 목적: /sys의 시스템 정보를 /home/haru2/sys에서도 접근 가능하게 함.

sudo mount --bind /tmp /media/hongyongjae/Database/cagongjoke/tmp
# /tmp 디렉토리를 /media/hongyongjae/Database/cagongjoke/tmp에 바인드 마운트합니다.
# 목적: /tmp의 임시 파일을 /media/hongyongjae/Database/cagongjoke/tmp에서도 접근 가능하게 함.
# 이게 없으면 apt-get 사용시 /tmp에 파일을 생성할수 없어서

#dated and the previous index files will be used. GPG error: #http://archive.ubuntu.com/ubuntu jammy InRelease: Couldn't create temporary file #/tmp/apt.conf.aRYMpu for passing config to apt-key
#W: Failed to fetch http://archive.ubuntu.com/ubuntu/dists/jammy/InRelease  #Couldn't create temporary file /tmp/apt.conf.aRYMpu for passing config to apt-key
#W: Some index files failed to download. They have been ignored, or old ones used #instead.

#에러가 뜰수 있음

sudo mount -t devpts devpts /home/haru2/dev/pts
# devpts 파일 시스템 타입으로 새로운 장치 파일 시스템을 /home/haru2/dev/pts에 마운트합니다.
# 목적: 의사 터미널 장치를 /home/haru2/dev/pts에서 사용 가능하게 함.
# 쉽게 말해 sudo 사용 가능하게 하는 명령임!

```

### 7. 사용자 홈 디렉토리 생성 및 설정 파일 복사
`chroot` 환경 내에 실제 사용자 홈 디렉토리를 생성하고, 필요한 설정 파일을 복사합니다.

```bash
sudo mkdir -p /home/woody/home/woody
sudo cp -v /etc/skel/.bashrc /home/woody/home/woody/
sudo cp -v /etc/skel/.profile /home/woody/home/woody/
sudo cp -v /etc/skel/.bash_logout /home/woody/home/woody/
sudo chown -R woody:woody /home/woody/home/woody
sudo chown root:root woody
sudo chmod 755 woody
```

### 8. `/etc/passwd` 파일 설정
`chroot` 환경 내에 `/etc/passwd` 파일을 설정하여 `woody` 사용자의 홈 디렉토리와 셸을 지정합니다.

```bash
sudo nano /home/woody/etc/passwd
```

다음 내용을 추가하거나 수정합니다:

```plaintext
woody:x:1000:1000:Woody,,,:/home/woody/home/woody:/bin/bash
```

### 9. SSH 데몬 설정
SSH 데몬의 설정 파일을 편집하여 `woody` 사용자가 로그인 시 자동으로 `chroot` 환경으로 들어가도록 설정합니다.

```bash
sudo nano /etc/ssh/sshd_config
```

파일의 끝에 다음 내용을 추가합니다:

```plaintext
Match User woody
    ChrootDirectory /home/woody
    AllowTcpForwarding no
    X11Forwarding no
    ForceCommand /bin/bash
```

SSH 데몬을 재시작합니다:

```bash
sudo systemctl restart ssh
```

### 10. `schroot` 환경 확인
설정을 마친 후 `schroot` 명령어를 사용하여 `woody` 사용자의 `chroot` 환경이 올바르게 설정되었는지 확인합니다.

```bash
sudo schroot -c woody
```

이제 프롬프트가 `woody@hostname:~$`와 같이 일반 사용자 셸 프롬프트처럼 표시되어야 합니다.


#### a. `woody` 사용자에게 `sudo` 권한 부여
`woody` 사용자가 `sudo` 명령어를 사용할 수 있도록 권한을 부여합니다.


```bash
echo "woody ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/woody
chmod 0440 /etc/sudoers.d/woody
```

이제 `woody` 사용자는 `chroot` 환경 내에서 `sudo` 명령어를 사용할 수 있습니다.



