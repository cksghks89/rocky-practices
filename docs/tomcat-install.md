0. 작업은 /root

1. tomcat9.0.89 다운로드
```sh
# wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.89/bin/apache-tomcat-9.0.89.tar.gz
```

3. 압축 풀기
```sh
# tar xvfz apache-tomcat-9.0.89.tar.gz
```

4. 설치
```sh
# mv apache-tomcat-9.0.89 /usr/local/poscodx
# cd /usr/local/poscodx/apache-tomcat-9.0.89
# ln -s apache-tomcat-9.0.89 tomcat
```

5. 설정(/etc/profile, 생략)


6. 포트 확인
```sh
   # vi /usr/local/poscodx/tomcat/conf/server.xml

   8080 open 확인
```

7. 실행
```sh
# /usr/local/poscodx/tomcat/bin/catalina.sh start
# ps -ef | grep tomcat
# ps -ef | grep java
```

8. 방화벽 (firewalld, 8080 포트) 열기
```sh
# vi /etc/firewalld/zones/public.xml
<zone>
...
	<service name="tomcat"/>
...
</zone>

# vi /usr/lib/firewalld/services/tomcat.xml

<?xml version="1.0" encoding="utf-8"?>
<service>
  <short>Tomcat</short>
  <description></description>
  <port protocol="tcp" port="8080"/>
</service>

# systemctl stop firewalld
# systemctl start firewalld
```

9. 브라우저로 접근 하기
```sh
   http://192.168.64.3:8080
```

10. 중지 시키기
```sh
# /usr/local/poscodx/tomcat/bin/catalina.sh stop
```

11. 서비스 등록 하기
```sh
   /usr/lib/systemd/system/tomcat.service 파일 생성 (서비스 스크립트 - 복붙해야 함)
   # systemctl enable tomcat
```

12. tomcat 서비스 실행/중지/재실행
```sh
   # systemctl start tomcat
   # systemctl stop tomcat
   # systemctl restart tomcat
```

13. tomcat manager 설정
```sh
   1) tomcat-users.xml 설정
      # vi /usr/local/poscodx/tomcat/conf/tomcat-users.xml

========================================================
<tomcat-users>
  . . .
  . . . 
  <role rolename="manager"/>
  <role rolename="manager-gui" />
  <role rolename="manager-script" />
  <role rolename="manager-jmx" />
  <role rolename="manager-status" />
  <role rolename="admin"/>
  <user username="admin" password="manager" roles="admin,manager,manager-gui, manager-script, manager-jmx, manager-status"/>

</tomcat-users>
========================================================
   2) /usr/local/poscodx/tomcat/webapps/manager/META-INF/context.xml
========================================================
 주석 처리
<Context>
 ....
</Context>

새로 다음내용 추가
<Context antiResourceLocking="false" privileged="true" docBase="${catalina.home}/webapps/manager">
  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="^.*$" />
</Context>

========================================================
```

14. tomcat 재시작
```sh
    # systemctl stop tomcat
    # ps -ef | grep tomcat
    # systemctl start tomcat
```

13. http://192.168.80.131/manager
