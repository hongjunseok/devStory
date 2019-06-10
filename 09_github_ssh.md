1. 터미널에서 등록된 키 확인
```
ls -al ~/.ssh

# 출력되는게 없다면 (ex> id_rsa.pub) 새로 만들자.
```


2. 키생성
```
$ ssh-keygen -t rsa -C “your_email@example.com”



비밀번호를 입력하라고 하면 비밀번호를 입력해준다.

Enter passphrase (empty for no passphrase):
Enter same passphrase again:

```

3.  새로운 키를 에이전트에 추가
```
$ eval “$(ssh-agent -s)”
$ ssh-add ~/.ssh/id_rsa
```


4. 생성된 키를 github 홈페이지에 등록한다.

- [Settings] --> [SSH and GPG keys] --> [New SSH key]

- 클립 보드에 ~/.ssh/id_rsa.pub의 내용을 복사

사용가능한 명령어
```
$ bcopy < ~/.ssh/id_rsa.pub 

$ cat ~/.ssh/id_rsa.pub 

$ vi ~/.ssh/id_rsa.pub
```
