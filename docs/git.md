# git



## git使用中遇到的问题



### OpenSSL SSL_read: Connection was reset, errno 10054

?> 把笔记上传到GitHub的时候，冒出了这个错误，原本还以为是网络问题，然后搜了一下，说是因为**服务器的SSL证书没有经过第三方机构的签署导致错误**。所以，重新全局配置一下就好了：

```shell
git config --global http.sslVerify "false"
```

