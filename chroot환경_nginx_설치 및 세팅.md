## 1. nginx 설치
```
# 일반설치와 차이없음
sudo apt-get install nginx x
```

## 2.필요 디렉토리 생성 
```
sudo mkdir -p /mnt/chroot/ubuntu/var/log/nginx
sudo mkdir -p /mnt/chroot/ubuntu/var/run
sudo mkdir -p /mnt/chroot/ubuntu/var/cache/nginx

```

## 확인 사항

1. 80포트 사용중이라면 /etc/nginx/site-enabled/default 파일 에서 포트 변경

## 실행

```
nginx # 수동실행 
#또는
nginx -c /etc/nginx/nginx.conf # 설정 파일 지정 수동실행 
```
