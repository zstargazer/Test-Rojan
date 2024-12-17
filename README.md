**如何打造个人的Trojan服务器**
1. 拥有一台境外的vps（oracle cloud）
   1.1 注册oracle cloud的服务器，可以参考“Youtube 零度解说”；
   1.2 新增“实例”，选择Ubuntu 22.04版本，并选择AMD的芯片；
   1.3 下载私钥并保存；
   1.4 创建实例后，保存IP4的地址；
   1.5 在入站规则中确认80，443端口是开放状态(0.0.0.0/0).

2. 下载FinalShell，安装X-UI界面
   2.1 文件夹图标，新建"SSL连接(Linux)"
   2.2 主机：oracle 实例的IP4地址，端口：22，认证方法：公钥，并添加从oracle下载保存的私钥，高级勾选“智能加速”。
   2.3 连接主机成功后，用sudo -i命令获取管理员权限。
   2.4 准备安装X-UI界面，可以参考https://www.youtube.com/watch?v=DZ1d_k9VgQY
   2.5 X-UI下载地址和视频不一样，使用：https://github.com/yonggekkk/x-ui-yg；使用回车，创建随机的用户名，密码，登录端口等信息；
   2.6 安装完成后，拍照记录下刚才生成的随机登录信息。
   2.7 回到vps中，X-UI生成的登录端口需要添加进vps的入站规则。源类型：CIDR，无状态：灰色，源CIDR：0.0.0.0/0，IP协议：TCP，目的地端口范围：“刚才X-UI生成的随机登陆端口”。
   2.8 使用https://IP4地址/X-UI随机登录端口/xxx，会显示vps中安装的X-UI登录界面，使用随机生成的用户名和密码登录X-UI
   2.9 在“节点列表”中“添加节点”，必要项：
     2.9.1 协议：vmess；启用：蓝色；监听IP：空；端口：443；到期时间：空；uuid：默认不动；额外ID：0；传输协议：ws；禁用不安全加密：灰色；AcceptProxyProtocol：灰色；路径：/123;
           请求头添加：名称Host，值：us.kg申请的域名；TLS（域名证书已申请）：蓝色；证书所指向的域名：us.kg申请的域名；证书方式：certificate file content; 公钥内容:------BEGIN CERTIFICATE-----;密钥内容：-----BEGIN PRIVATE KEY-----;
           Alpn：h2, http/1.1; sniffing:蓝色。

3. 申请域名
   3.1 通过us.kg网站申请，可以参考“Youtube 零度解说”；
     3.1.1 申请了“zxxzer.us.kg"作为个人网站的域名；
   3.2 通过Cloudflare解析域名，将"zxxzer.us.kg"和实例的IP4地址绑定，并开启代理；
   3.3 将Cloudflare DNS的记录里，两个NS类型的值复制到us.kg对应的“Name Server 1 & 2”中；
   3.3 通过Cloudflare的SSL/TLS的源服务器申请证书，申请后，不要关闭私钥和证书页面，保存两个证书内容，从“-----BEGIN xxx-----”到“-----END xxx-----”完整复制；
   3.4 将复制的两份证书导入2.9.1的“公钥内容” & “私钥内容”；
   3.5 保存后，点击“操作”，生成二维码，复制该二维码，或在手机端扫面该二维码。

4. v2rayN
   4.1 下载电脑端v2rayN，“服务器”中点击“从剪切板导入分享链接”，自动导入连接，重启服务后，测试服务器真延时，如显示“-1”则需要排查问题（可尝试FinalShell中同步时间）。
