***
建立起ftp连接的时候其实建立了两个tcp连接，21号端口用于传送控制信号，20号端口用于传输数据

***
## Diff between active mode and passive mode

***注意这里的主动与被动都是针对服务器而言的***

**Benefit of active FTP:** it increase FTP server security

![active](https://securitywing.com/active-vs-passive-ftp-mode-which-one-is-more-secure/active-ftp-mode/)

**Benefit of passive FTP:** it requires less configuration changes on client  computer.
![pasv](https://securitywing.com/active-vs-passive-ftp-mode-which-one-is-more-secure/ftp-passive-mode/)

然而这两个模式都不安全，都没使用到SSL
