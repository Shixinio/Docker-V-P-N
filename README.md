# Docker-V-P-N
**利用Docker一键安装Shadowsocks搭建VPN**

服务器： CentOS7 x64  最低配置即可

**VPS的设置以及VPN的搭建－shadowsorck**

**安装Docker**

~~~shell
yum install docker -y
~~~



**启动Docker**

~~~shell
service docker start
~~~



![用Docker搭建VPN (2)](https://cdn.shortpixel.ai/client/q_glossy,ret_img,w_529,h_99/https://vkuajing.net/wp-content/uploads/2019/03/%E7%94%A8Docker%E6%90%AD%E5%BB%BAVPN-2.png)

~~~shell
chkconfig docker on
~~~



![用Docker搭建VPN (3)](https://cdn.shortpixel.ai/client/q_glossy,ret_img,w_583,h_115/https://vkuajing.net/wp-content/uploads/2019/03/%E7%94%A8Docker%E6%90%AD%E5%BB%BAVPN-3.png)

**安装 Shadowsocks 的 [VPN Docker 镜像](https://github.com/Shixinio/shadowsocks-docker)**

~~~shell
docker pull imhang/shadowsocks-docker
~~~



![VPN搭建教程 bbr加速 (1)](https://cdn.shortpixel.ai/client/q_glossy,ret_img,w_581/https://vkuajing.net/wp-content/uploads/2020/01/VPN%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B-bbr%E5%8A%A0%E9%80%9F-1.png)

 **运行镜像**

~~~shell
docker run -d* *--restart**=always -e “SS_PORT=**yourport**” -e “SS_PASSWORD=**yourpassword**” -e “SS_METHOD=**salsa20**” -e “SS_TIMEOUT=600” -p 443:443 -p 443:443/udp* *--name* *ssserver imhang/shadowsocks-docker
~~~



***只需要修改端口和密码，其他默认即可***

~~~shell
- SS_PASSWORD= 密码，比如案例的密码是 yourpassword 
- SS_METHOD 是加密方式，建议用一些不常见的比如 salsa20 或者 chacha20 等，更不容易被识别
- SS_PORT 这里是**服务器端口号**，比如案例的端口号是yourport
~~~



***到这里其实已经搭建成功了，已经可以正常连接科学上网，但是如果你要开始BBR加速，要进行下一步***



**如果你的服务器配置比较高，也许你不需要BBR加速已经完全可以满足要求，所以这一步是可选**

~~~shell
yum update
~~~



![VPN搭建教程 bbr加速 (3)](https://cdn.shortpixel.ai/client/q_glossy,ret_img,w_642/https://vkuajing.net/wp-content/uploads/2020/01/VPN%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B-bbr%E5%8A%A0%E9%80%9F-3.png)

选择Y 继续

![VPN搭建教程 bbr加速 (4)](https://cdn.shortpixel.ai/client/q_glossy,ret_img,w_345/https://vkuajing.net/wp-content/uploads/2020/01/VPN%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B-bbr%E5%8A%A0%E9%80%9F-4.png)



~~~shell
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && chmod +x bbr.sh && ./bbr.sh
~~~



粘贴代码，按Enter

![VPN搭建教程 bbr加速 (5)](https://cdn.shortpixel.ai/client/q_glossy,ret_img,w_1070/https://vkuajing.net/wp-content/uploads/2020/01/VPN%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B-bbr%E5%8A%A0%E9%80%9F-5.png)

**然后服务器会返回信息，你看到的应该是这个，**按任意键继续后**，会安装新的内核**

**安装完之后 输入Y 点 enter 重启VPS服务器**

**重启后VPS会自动断开连接，等几秒重新连接即可，到这里BBR加速成功。**你会发现上网

![VPN搭建教程 bbr加速 (8)](https://cdn.shortpixel.ai/client/q_glossy,ret_img,w_453/https://vkuajing.net/wp-content/uploads/2020/01/VPN%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B-bbr%E5%8A%A0%E9%80%9F-8.png)速度明显加快！！！**

**一般只要按照步骤来不会出现问题，但是如果你想检测是不是成功开启了BBR，你可以用如下代码**

~~~shell
uname -r

sysctl net.ipv4.tcp_available_congestion_control 【返回 reno cubic bbr】

sysctl net.ipv4.tcp_congestion_control【返回 bbr】

sysctl net.core.default_qdisc【返回 fq】

lsmod | grep bbr【返回 tcp_bbr模块】
~~~



![VPN搭建教程 bbr加速 (9)](https://cdn.shortpixel.ai/client/q_glossy,ret_img,w_658/https://vkuajing.net/wp-content/uploads/2020/01/VPN%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B-bbr%E5%8A%A0%E9%80%9F-9.png)

