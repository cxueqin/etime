
>“君子务本，本立则道生。”在信息世界，所有的应用都是构筑在操作系统上，没有操作系统安全，应用的安全就无从谈起。随着国家建立建立网络强国的战略推进，操作系统作为基础设施，其安全性至关重要。

1. 基础理论
===========
1.1 操作系统的地位
-----------------
* 一个平台，应用程序运行其上
* 一个管理者，统筹分配各种资源
资源包括两大类：硬件资源和信息资源。其中，硬件资源分为处理器CPU、内存、外部设备等；信息资源则分为程序和数据。
介乎应用与硬件之间，对应用安全有决定性作用

1.2 操作系统的安全机制
---------------------
* 鉴别用户身份

用户要使用应用，操作系统首先会检查用户的身份，这个过程叫鉴别。我们通常通过输入用户名和密码的方式实现身份的认证，随着技术的不断的发展，操作系统还支持其他多种鉴别技术。例如Windows支持通过智能卡方式鉴别，这个在一些大企业中很方便；支持通过人脸鉴别用户，例如Surface中的Windows系统就在使用。就在昨天，刚刚发布的新一代MacBook Pro就继承了TOUCH ID指纹鉴别，用户只要像使用iPhone一样，按一下指纹即可进入系统。

* 访问控制

访问控制的根本就是防止非法用户进入系统或者合法用户非法使用系统资源。在前面的文章中有介绍过，访问控制分为3种：用户自主访问控制、强制访问控制和基于角色的访问控制。在2000年以前，像windows系统还是基于用户自主的方式，安全要靠用户自己，要购买杀毒软件，要做各种的设置，应用程序拥有很大的权限，访问系统资源约束不大，所以那时候windows系统的蓝屏死机是很普遍的。2000年后，windows系统就越来越重视安全性，默认有一套内部的访问控制规则，应用必须运行在规则之下，不可预约。这种控制是操作系统强制的，即为强制访问控制。最新的操作系统，有个技术叫SANDBOX（沙盒），通过它可以实现应用与系统的安全隔离，在苹果的IOS和windows系统中皆有应用。基于角色的访问控制，在用户权限管理中有大量的应用。

* 最小特权管理

最小特权意味着操作系统只给予用户“必不可少”的特权，保证能够完成需要的任务或操作。这种思想是当代操作系统安全设计的核心。当然，因为权限是最小的，也限制了应用的一些能力。往往，灵活与安全性要能够同时达到很难，如何平衡两者考验工程师的智慧。

* 信道保护

可信通路机制一般以安全键(Security Attention Key)为基础实现的，例如Windows系统种用户同时按下<CTRL><ALT><DEL>三键，Windows会终止所有用户进程，切换到锁定、用户切换、注销、任务管理的界面上。这就是一种信道保护。

* 安全审计

操作系统的审计机制就是对系统中有关安全的活动进行记录、检查以及审核，目的是检查和发现非法用户对系统的入侵，以及合法用户的误操作。审计一般是独立的过程，操作系统要保护审计数据，限制未经授权的用户访问。

* 内存存储保护，进程隔离

一般来说应用对应一个或若干个进程，一个进程的内存数据只能由本进程使用，必须限制其他进程的访问。良好的内存保护隔离，也有助于预防诸如缓冲区溢出导致的数据恶意修改风险。

* 文件系统保护

主要有4个方面：分区、文件共享、文件备份。
分区：一个硬盘可以分为若干个不同分区，每个分区都有独立用途，例如将日志当作做一个分区划出，日志分区的数据即使塞满了，也不会影响其它分区的正常读写，保证   安全性。再比如将用户数据独立做分区，可单独加密，非法用户就无法拷贝分区中的数据。
文件共享：多人共用一台计算机时，须可以控制文件的共享权限，规定谁可读，谁可写。
文件备份：例如IOS系统，它就是提供iCloud的云端自动数据备份，保障用户的照片、通讯录、日程等数据的安全。

2. Windows安全实操
==================
2.1 安全配置准备工作
-------------------
* 不要只使用一个分区，须将系统和用户数据分开
* 及时安装系统安全补丁，微软Windows 10的安全更新做了整合，将原来分散的安全固定整个为一个，推送到用户桌面
* 定期检查安全更新，需要的话，还可设置自动更新

2.2 账号安全设置
---------------
默认管理账户adminstrator更名
密码需要足够复杂，消除弱口令
密码长度由最小值，必须有包含特殊字符
设置密码最短使用期限
设定密码最长使用期限

P2.png
设置系统账号策略
应对暴力破解，设置账号锁定时间
应对暴力破解，设置账号锁定失败次数阈值
应对暴力破解，设置账号解锁超时时间
用户权限分配，重点关注这些配置
从网络访问这台计算机
拒绝从网络访问这台计算机
管理审核和安全日志
从远程系统强制关机

P3.png
设置唤醒密码
设置唤醒计算机时需要密码
设置屏幕保护恢复时需要密码
2.3 关闭自动播放功能
Windows系统为了方便用户，当光盘或者光盘插入计算机时，系统会自动运行autorun.exe程序。攻击者就通过这个机制，把攻击程序制作成autorun.exe，轻而易举的攻入系统。所以，关闭自动播放功能可有效的阻断U盘病毒的传播。P4.png
2.4 远程访问控制
启用自带防火墙,注意Windows防火墙是个人级别的防火墙，功能是有限的，对个人还是适用的
空会话连接控制，空连接会话可能导致信息泄漏
关闭管理共享，可能导致远程文件修改
2.5 本地安全策略
交互式登录：无须按Ctrl+Alt+Del，设置为已禁用
交互式登录：不显示最后的用户名，设置为已启用
将CD-ROM 的访问权限仅限于本地登录的用户，设置为已启用
网络访问：不允许SAM账号的匿名枚举，设置为已启用
网络访问：可匿名访问的共享，删除策略设置里的值
网络访问：可匿名访问的命名管道，删除策略设置里的值
网络访问：可远程访问的注册表路径，删除策略设置里的值
P5.png
2.6 服务运行安全
关闭不必要的服务，例如Remote Desktop Service远程桌面共享等
控制服务权限，比如文件共享的权限
2.7 第3方安全软件
安装病毒防护软件。尽管操作系统有做基础层面的安全防护，但对于日新月异的病毒还是心有余而力不足。360、金山等杀毒软件厂家也给用户提供的不错的防护，建议安装。
配置额外的系统安全保护软件，主机入侵检测软件等
3. Linux系统安全实操
3.1 安全配置准备工作
使用官方发布的系统安全包。Linux的开源软件开放性，系统各个软件包源码皆可从网络上下载。理论上任何人都可以重新制作系统安全包，一些不法之徒若将恶意程序埋入系统，这将导致严重的安全问题。故安装时务必要检查发布的文件来源，检查一下安装文件的MD5或SHA哈希值，更好的是校验一下其GPG签名，一些发行版有带。
分区挂载重要目录。 根目录（/）、用户目录（/home）、临时目录（/tmp）等最好分到不同的磁盘分区，区别对待。
自定义安装的软件包，不需要的软件包尤其是服务程序包尽量不安装。
及时更新系统和软件包补丁，像CentOS系统采用yum软件包管理器，有图形或命令行的更新接口，应该定期更新。
3.2 账号与口令安全
检查、清除系统中多余账号和组.可检查/etc/passwd和/etc/group文件
对于特殊保留的系统伪账户，可以设置锁定登录
禁用root之外的超级用户
检查用户的口令复杂度,严格禁止空口令
设置账户锁定登录失败锁定次数、锁定时间
修改账户超时值，设置自动注销时间
禁止使用root登录系,只允许普通用户登陆，然后通过su命令切换到root
原则上禁止以root运行其他用户的或不熟悉的程序
3.3 系统服务配置
禁止 危险的网络服务，如telnet、NFS、echo、RPC等
关闭非必需的网络服务，如talk、ntalk等
保证使用的服务软件是安全的版本，定期检查软件补丁情况

3.4 远程登录安全
禁止 telnet,使用SSH进行管理
限制能够登录本机的IP地址
禁止root用户远程登录
限定信任主机
修改banner标语信息，隐藏操作系统、版本等信息
3.5 文件和目录安全
保护重要的文件目录，限制用户访问
临时文件不应该有执行权限
可以根据要求设置新文件的默认访问权限，如仅允许文件属主访问，不允许其他人访问
检查文件的SUID、SGID、STICKY BIT，防止一些可执行文件的恶意提权和目录文件的恶意删除
3.6 系统日志配置
启用syslog服务
配置日志存储策略，检查其对日志存储空间的大小和时间的设置
检查syslog.conf配置，确保设定合适的日志容量
推荐日志文件统一归集管理
3.7 使用安全软件
启用主机防火墙。Linux下的防火墙iptables功能强大，可以实现各种访问控制。建议开启主机自带的防火墙，默认开机即启动。
使用官方的安全软件包，如openssh、openssl等
如有需要，可使用snort入侵检测系统
4. 回顾总结
操作系统是所有IT系统的基石，它的安全性决定了上层应用的安全性，需加以充分的研究与重视。
操作系统的安全保障是有章法可寻的，通过windows和Linux两大系统比较可以看到许多措施都是类似的，细节也有区别。应当在产品与项目中结合实际实施相关的安全配置。
操作系统的安全性也是不断在完善的，需要加以持续的关注与研究。
