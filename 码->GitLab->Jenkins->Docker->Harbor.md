# 码->GitLab->Jenkins->Docker->Harbor(针对Ubuntu 18.04 LTS 服务器版)

> ubuntu 18.04 LTS 服务器版 默认不开启网络端口的DHCP 需要执行命令 sudo dhclient 端口

不知为何ubuntu网速特慢，[加速](https://blog.csdn.net/weixin_43469680/article/details/89019945)

## GitLab安装
 网上随便搜索一个教程跟着安装即可
> [安装gitlab教程示例](https://www.cnblogs.com/xiaojf/p/11113363.html)

注意事项：

> 修改配置文件 /etc/gitlab/gitlab.rb 启动端口后也需要修改external_url添加上端口号（不然Clone with HTTP提示将不正确）
> 
> root用户登录gitlab之后setting->network->Outbund requests勾选Allow requests to the local network from web hooks and services（如果你需要使用本地的web hook,不然添加本级的web hook会报错）

## 安装Jenkins
