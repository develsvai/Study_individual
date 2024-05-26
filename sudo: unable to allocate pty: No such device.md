`sudo: unable to allocate pty: No such device` 오류는 `chroot` 환경 내에서 `devpts` 파일 시스템이 올바르게 마운트되지 않아서 발생합니다. `chroot` 환경 내에서 `sudo`와 같은 명령어를 사용할 때는 Pseudo Terminal (PTY)를 할당할 수 있어야 합니다. 이를 해결하기 위해서는 `devpts` 파일 시스템을 마운트해야 합니다.

다음 단계에 따라 문제를 해결할 수 있습니다.

### 1. `chroot` 환경 내에서 필요한 파일 시스템 마운트
`chroot` 환경 내에서 필요한 파일 시스템을 마운트합니다. 이는 `/dev`, `/proc`, `/sys` 및 `devpts` 파일 시스템을 포함합니다.

```bash
sudo mount --bind /dev /home/woody/dev
sudo mount --bind /proc /home/woody/proc
sudo mount --bind /sys /home/woody/sys
sudo mount -t devpts devpts /home/woody/dev/pts
```

### 2. `schroot` 환경으로 진입 및 확인
위의 파일 시스템을 마운트한 후, `schroot` 환경으로 진입하여 `sudo` 명령어가 작동하는지 확인합니다.

```bash
sudo schroot -c woody
```

`chroot` 환경 내에서 `sudo` 명령어를 실행하여 확인합니다.

```bash
sudo ls
```

이제 `sudo` 명령어가 올바르게 작동해야 합니다.

### 3. 파일 시스템 마운트를 자동화
위의 마운트 명령어를 `schroot` 설정 파일에 추가하여 자동으로 파일 시스템이 마운트되도록 설정할 수 있습니다.

#### a. `/etc/schroot/default/fstab` 파일 편집
`/etc/schroot/default/fstab` 파일을 편집하여 필요한 파일 시스템을 추가합니다. 해당 파일이 없으면 생성합니다.

```bash
sudo nano /etc/schroot/default/fstab
```

다음 내용을 추가합니다:

```plaintext
# /etc/schroot/default/fstab: static file system information.
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
/dev        /dev        none    rw,bind         0       0
/proc       /proc       none    rw,bind         0       0
/sys        /sys        none    rw,bind         0       0
devpts      /dev/pts    devpts  defaults        0       0
```

이 설정은 `schroot` 환경이 시작될 때 자동으로 파일 시스템을 마운트합니다.

### 4. SSH 데몬 설정
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

### 요약
1. `chroot` 환경 내에서 필요한 파일 시스템을 마운트합니다 (`/dev`, `/proc`, `/sys`, `devpts`).
2. `schroot` 환경으로 진입하여 `sudo` 명령어가 작동하는지 확인합니다.
3. 파일 시스템 마운트를 자동화하기 위해 `/etc/schroot/default/fstab` 파일을 설정합니다.
4. SSH 데몬 설정을 업데이트하여 `woody` 사용자가 로그인 시 `chroot` 환경으로 진입하도록 설정합니다.

이 단계를 통해 `chroot` 환경 내에서 `sudo` 명령어가 올바르게 작동하도록 설정할 수 있습니다. 추가로 궁금한 사항이나 다른 설정이 필요하면 말씀해 주세요.
