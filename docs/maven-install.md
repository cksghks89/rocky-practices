0. 작업 디렉토리
   /root

1. 다운로드
```sh
# wget https://dlcdn.apache.org/maven/maven-3/3.9.6/binaries/apache-maven-3.9.6-bin.tar.gz
```

2. 압축 풀기
```sh
# tar xvzf apache-maven-3.9.6-bin.tar.gz
```

3. 설치
```sh
# mv apache-maven-3.9.6 /usr/local/poscodx/
# cd /usr/local/poscodx
# ln -s apache-maven-3.9.6/ maven
```

4. 설정(/etc/profile)
```sh
# maven
export PATH=$PATH:/usr/local/poscodx/maven/bin
```
