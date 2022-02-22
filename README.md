# 아피치 웹서버 2.2 -> 2.4 업그레이드 

## 1. 아피치 웹서버 업그레이드 버전 정보
```
- ASIS Enviroments : 
Apache httpd 2.2.22(jws-2.0 by redhat)
Tomcat Connector 1.2.41(jws-2.0 by redhat)

- TOBE Enviroments : 
Apache httpd 2.2.52(latest version of 2022-02)
Tomcat Connector 1.2.48(latest version of 2022-02)
```

## 2. 아피치 웹서버 설치
Ansible Playbook 을 이용한 자동설치 

```
```

## 3. 아파치 웹서버 업그레이드
1. Apache httpd 서버 표준 설정 템플릿 구성
2. [ASIS] 설정 정보 분석후 [TOBE] 설정으로 이전
3. [TOBE] Apache httpd 서버 기동 후 문제점 확인 및 기본 연동 검증

```
```

### 3.1 주요 전환 사항
> 1. 사용 Module 확인 및 표준화(Proxy,Rewrite,SSL,Expire,Wstunnel,...)
```
LoadModule proxy_module /opt/jboss/httpd_2.4/modules/mod_proxy.so
...
```
> 2. Listen 포트 확인 전환
```
Listen 80
```
> 3. MPM 설정 확인 전환
```
<mpm_worker_module>
ThreadLimit 60
ServerLimit 10
StartServers 5
MinSpareThreads 120
MaxSpareThreads 240
ThreadPerChild 60
MaxRequestWorkers 600
MaxConnectionPerChild 0
</mpm_worker_mopdule>
```
> 4. VirtualHost 확인 전환
```
<VirtualHost *:80>
...
</VirtualHost>
```
> 5. WAS 연동 설정(JKMount,Uriworkermap,...)
```
JKMount /*.jsp wlb
```
> 6. 심볼릭 링크 허용(+FollowSymlink)
```
Otions ... +FollowSymlink
```
> 7. 에러 처리페이지 확인(ErrorDocument)
```
ErrorDocument 400 "[400 Error]"
```
> 8. 기본 Charset 설정(AddDefaultCharset UTF-8,EUC-KR,Off)
```
AddDefaultCharset UTF-8
```
> 9. 기본 사용자,그룹 설정(User,Group)
```
User daemon
Group daemon
```

> 10. 기본 설정(Timeout,KeepAlive,ServerToken,...)
```
Timeout 300
KeepAlive On
MaxKeepAliveTimeout 2000
ServerTokens Prod
...
```

> 11. Third Party Module(Perl 컴파일)
``` 
# https://perl.apache.org/download/ 다운로드
perl MakeFile.PL MP_APXS=/opt/jboss/httpd_2.4/bin/apxs MP_APR_CONFIG=/usr/local/apr/bin/apr-1-config MP_APU_CONFIG=/usr/local/apr-util/bin/apu-1-config
make && make install
# LoadModule on httpd.conf
LoadModule perl_Module /opt/jboss/httpd_2.4/modules/mod_perl.so
```

### 3.2 이슈 사항

> 1. Proxy HTTPS 연동시 에러 발생
```
Error during SSH Handshake with remote server
SSLProxyEngine on
SSLProxyVerify none 
SSLProxyCheckPeerCN off
SSLProxyCheckPeerName off
SSLProxyCheckPeerExpire off

```

## 4. 아파치 웹서버 실서버 이전
1. [ASIS] 서버의 배포 중단
2. [ASIS] 서버 SHUTDOWN
3. [TOBE] 서버 기동 후 기존 운영 서버의 IP 로 변경
4. 전환 서비스 확인

```
```

### 4.1 이슈 사항

```
```

