#### !chrrot 환경을 root로 접근할것!

chroot 환경에서 Docker를 설치하는 과정은 다소 복잡할 수 있지만, 다음 단계들을 따라 설치할 수 있습니다. chroot 환경 내에서 Docker를 제대로 실행하려면 몇 가지 추가 설정이 필요합니다.

### 1. chroot 환경으로 이동
먼저 chroot 환경으로 이동합니다. chroot 환경이 `/home/woody`에 있다고 가정,
이동하지 말것!,

```bash
sudo chroot /home/woody
```

### 2. 필요한 디렉토리 마운트
chroot 환경 내에서 Docker를 실행하려면 몇 가지 중요한 파일 시스템을 마운트해야 함,
단 확인결과 도커는 메인 시스템과 완전 분리는 어려운것으로 확인됨 

완전 분리를 위해서는 분리된 환경에 시스템파일을 복제하고 아래 명령을 통해 재마운트하는 과정이 필요하지만 
```
mount -t proc /proc /proc
mount --rbind /sys /sys
mount --rbind /dev /dev
mount --rbind /run /run
```
해당 명령을 사용하면 컨테이너를  실행하는 과정에서 
```Attempting next endpoint for pull after error: failed to register layer: remount /, flags: 0x84000: invalid argument```
위의 에러가 뜨고 현재는 원인을 알수없음 

따라서 도커가 사용하는 시스템 파일은 아래 명령을 통해  메인의 시스템 파일을 바인딩하여 사용 즉 메인 시스템에 설치된 도커 와 프로세스 를  공유함.

#### !이 과정은 메인 시스템에서 sudo 권한으로 이루어져야함!(재부팅시 리마운트)
```bash
sudo mount --rbind /sys /home/woody/sys
sudo mount --rbind /proc /home/woody/proc
sudo mount --rbind /dev /home/woody/dev
sudo mount --rbind /run /home/woody/run
sudo mount --rbind /sys/fs/cgroup /media/hongyongjae/Database/cagongjoke/sys/fs/cgroup
```

### 3. Docker 설치 !ubuntu22.04 기준
이제 chroot 환경 내에서 Docker를 설치합니다. 아래 명령어는 Ubuntu에서 Docker를 설치하는 예시입니다.

#### Docker 설치 준비
```bash
apt-get update
//기본 gnupg2 는 사용되지 않은 대체 패키지 gnupg를 사용
apt-get install -y apt ca-certificates curl gnupg software-properties-common

```

#### Docker GPG 키 추가
```bash
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg


```

#### Docker 저장소 추가
```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

```

#### Docker 설치
```bash
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io

```

#### Docker 데몬 실행
```bash
dockerd &
```

### 5. Docker 사용
이제 Docker를 사용할 수 있습니다. 예시로 hello-world 이미지를 실행해봅니다.

```bash
docker run hello-world
```
