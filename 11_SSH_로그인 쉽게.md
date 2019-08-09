## AWS EC2 pem key 다운로드
```
> cp 이름.pem ~/.ssh
> chmod 700 이름.pem
> touch config
# DFANG
Host 퍼블릭 주소
        IdentityFile ~/.ssh/이름.pem
        User 접속 계정

```
