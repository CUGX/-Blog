# Linux查看组用户信息

## 1、groups

查看当前用户所属组：

```shell
#查看系统当前登录的用户所属的用户组
groups

#查看特定用户所属用户组
groups username
```



## 2、id

```shell
#查看系统当前登录的用户所属的用户组
id

#查看特定用户所属用户组
id username
```



## 3、所用用户组信息

```shell
#查看系统所用用户组信息
vi /etc/group

#通过/etc/group查询某用户属组
cat /etc/group | grep username
```







