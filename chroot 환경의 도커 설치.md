# !!chrrot 환경을 root로 접근할것!


chroot 환경에서 Docker를 설치하는 과정은 다소 복잡할 수 있지만, 다음 단계들을 따라 설치할 수 있습니다. chroot 환경 내에서 Docker를 제대로 실행하려면 몇 가지 추가 설정이 필요합니다.

### 1. chroot 환경으로 이동
먼저 chroot 환경으로 이동합니다. chroot 환경이 `/home/woody`에 있다고 가정합니다.

```bash
sudo chroot /home/woody
```

### 2. 필요한 디렉토리 마운트
chroot 환경 내에서 Docker를 실행하려면 몇 가지 중요한 파일 시스템을 마운트해야 합니다.

```bash
mount -t proc /proc /proc
mount --rbind /sys /sys
mount --rbind /dev /dev
mount --rbind /run /run
```

### 3. Docker 설치
이제 chroot 환경 내에서 Docker를 설치합니다. 아래 명령어는 Ubuntu에서 Docker를 설치하는 예시입니다.

#### Docker 설치 준비
```bash
apt-get update
apt-get install -y apt-transport-https ca-certificates curl software-properties-common
```

#### Docker GPG 키 추가
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

```

#### Docker 저장소 추가
```bash
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

#### Docker 설치
```bash
apt-get update
apt-get install -y docker-ce
```

### 4. Docker 데몬 실행
chroot 환경 내에서 Docker 데몬을 실행해야 합니다. 이를 위해 systemd를 사용할 수 없으므로 직접 데몬을 실행해야 합니다.

```bash
dockerd &
```

### 5. Docker 사용
이제 Docker를 사용할 수 있습니다. 예시로 hello-world 이미지를 실행해봅니다.

```bash
docker run hello-world
```

### 전체 절차 요약

1. chroot 환경으로 이동:
   ```bash
   sudo chroot /home/woody
   ```

2. 필요한 디렉토리 마운트:
   ```bash
   mount -t proc /proc /proc
   mount --rbind /sys /sys
   mount --rbind /dev /dev
   mount --rbind /run /run
   ```

3. Docker 설치 준비:
   ```bash
   apt-get update
   apt-get install -y apt-transport-https ca-certificates curl software-properties-common
   ```

4. Docker GPG 키 추가:
   ```bash
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
   ```

5. Docker 저장소 추가:
   ```bash
   add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
   ```

6. Docker 설치:
   ```bash
   apt-get update
   apt-get install -y docker-ce
   ```

7. Docker 데몬 실행:
   ```bash
   dockerd &
   ```

8. Docker 사용:
   ```bash
   docker run hello-world
   ```

이 단계를 통해 chroot 환경 내에서 Docker를 설치하고 사용할 수 있습니다. 그러나 chroot 환경에서 Docker를 사용하는 것은 다소 제한적일 수 있으며, 추가적인 설정이 필요할 수 있습니다.
