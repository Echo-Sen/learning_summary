# Linux

## 准备工作

### Linux 内核

- 内核: 提供了Linux 系统的主要功能，如硬件调度管理的能力

- 系统级应用程序 +  内核 = 操作系统

- Linux 源码获取 [The Linux Kernel Archives](https://www.kernel.org/)

### 虚拟机安装

- vmware 下载地址[Windows 虚拟机 | Workstation Pro | VMware | CN](https://www.vmware.com/cn/products/workstation-pro.html)
- 许可证 [虚拟机vmware17密钥 vmware workstation17许可证密钥_IT备忘录 (itmemo.cn)](https://www.itmemo.cn/html/1204.html)
- 检查 vmware vmwnet1和wmnet8的两个网卡是否启用 ncpa.cpl快捷检查

- Linux 版本 centos7.6
- 配置 Linux 

### 连接 Linux虚拟机

- mobaxterm

### 了解 WSL

- 在 windows 系统中开启 wsl
- 在应用商店 下载 乌班图(ubuntu) 系统
- 使用 Windows Terminal 操作 乌班图系统

### 虚拟机快照

虚拟机损坏快速恢复的方法（存档）



## 起步

### Linux 系统目录结构

- Linux 没有盘符概念，只有一个根目录 / 
- Window 用 \ 表示层级， Linux 用 / 表示层级

### linux 命令

#### 命令格式

![image-20230111190240182](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230111190240182.png)

#### ls

- ls 查看当前目录文件
- ![image-20230111191726536](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230111191726536.png)
- -a -l -h
- -a查看全部包括隐藏文件 -l竖向排列 -h文件内存大小
- -al全部并竖向 -lh竖向并列出文件大小（-h必须和-l一起使用）

#### cd 

- ![image-20230111191857632](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230111191857632.png)

#### pwd 查看当前工作目录

- pwd 查看当前工作目录
- ![image-20230111191943428](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230111191943428.png)

#### mkdir 创建目录

- 语法：![image-20230111193323604](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230111193323604.png)

- -p 表示自动创建不存在的父目录，适用于创建连续多层级的目录

#### touch 创建文件

- 语法：![image-20230111193745058](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230111193745058.png)
- 可以通过touch命令创建文件

#### cat 查看文件内容

- 语法：![image-20230111194051147](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230111194051147.png)

#### more 查看文件内容

- 语法：![image-20230111194332917](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230111194332917.png)
- 与 cat 不同的是可以 分页展示内容

#### cp 复制文件文件夹

- 语法：![image-20230111194557340](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230111194557340.png)
- -r选项，可选，用于复制文件夹使用，表示递归
- 参数1，Linux路径，表示被复制的文件或文件夹
- 参数2，Linux路径，表示要复制去的地方
- 复制 文件夹 必须要用-r

#### mv移动文件或文件夹

- 语法：![image-20230111200334009](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230111200334009.png)

- ![image-20230111200408413](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230111200408413.png)

- ![image-20230111200418236](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230111200418236.png)
- ![image-20230111200426339](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230111200426339.png)

#### rm删除文件、文件夹

- 语法：![image-20230111200732723](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230111200732723.png)

- -r 同 cp 命令一样删除 文件夹 必须要用
- -f 强制删除 不会弹出确认提示
  - 普通用户删除不会弹出 确认提示，root 管理员删除内容才会有提示
  - 所以一般不会用到
- 参数为删除目标文件的路径
- su - root 切换到 root 管理员身份
- exit 退出 root 管理员身份
- rm 支持以 * 为通配符模糊匹配
  - *表示通配符删除全部
  - *test 表示删除以 test 结尾的内容
  - test* 表示删除以 test 开头的内容
  - *test * 表示删除包含 test 的内容

**<font color="red">注意：rm -rf / 或 rm -rf /* 等同于格式化计算机 谨慎使用</font>**



#### which 查找命令的位置

- 语法：![image-20230111203534427](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230111203534427.png)
- ![image-20230111203546036](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230111203546036.png)

#### find 按文件名查找文件

- 语法![](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230111211620167.png)

- 通配符模糊查找 同 which

#### find 按文件大小查找文件

- 语法![image-20230111211815073](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230111211815073.png)

- +-表示 大于 或 小于
- n 表示大小
- kmg表示单位 
- 如：查找小于10KB的文件： find / -size -10k

#### grep 关键字过滤

- 语法：![image-20230111212824576](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230111212824576.png)
- -n 是可选参数 是否展示位于哪一行
- 关键字用 “” 包裹

#### wc

- 语法：![image-20230111213208432](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230111213208432.png)
- 选项，-c，统计bytes数量
- 选项，-m，统计字符数量
- 选项，-l，统计行数
- 选项，-w，统计单词数量
- 参数，文件路径，被统计的文件，可作为内容输入端口

#### | 

- 管道符将左边的命令结果作为右边的输入

#### echo 输出

- 语法：![image-20230112125048797](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230112125048797.png)
- 建议 输出内容都使用 双引号包括
- 输出命令使用命令 反引号 
- 重定向符 >覆盖写入到符号右侧指定文件 >>追加写入到符号右侧指定文件 

#### tail 查看文件尾部内容

- 语法：![image-20230112173627030](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230112173627030.png)
-  -f 持续跟踪 -num 查看尾部多少行默认为10行

#### vi/vim 写入文件

- vim 是 vi 升级版用 vim 即可
- 语法：![image-20230112174746713](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230112174746713.png)
- 输入命令后有三种 运行模式
  - 命令模式 键盘快捷键控制文件内容
  - 输入模式 一般 i 进入 esc 回退
  - 底线命令模式 :wq 保存并退出

#### man 查看命令帮助

- 语法：man 命令
- 可将命令手册 输入到文件翻译 man ls >ls -man.txt

- 命令 -help 查看命令帮助

### Linux用户和权限

#### root 用户

- 操作系统采用 **多用户管理** 的模式进行权限管理

#### su 切换用户

- 语法：![image-20230112181100264](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230112181100264.png)
- -表示是否在切换用户后 加载环境变量 建议带上
- 省略用户名 默认切换 root 用户

#### exit 回退管理用户

- exit 或 快捷键 ctrl + d

#### sudo 赋予 最高权限

-  语法：![image-20230112182054223](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230112182054223.png)

- sudo 为的是防止 一直处于 root 用户 可能会做出损害系统的操作

- 需要为普通用户 配置 sudo 认证

  - 切换到 root 用户

  - 执行 visudo 命令， 会自动通过 vim 打开 /etc/sudoers

  - 在文件最后添加 ![image-20230113141039096](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230113141039096.png)

    其中最后的NOPASSWD:ALL 表示使用sudo命令，无需输入密码

    最后通过 wq 保存

  - 切换回 普通用户


### 用户 用户组

#### 用户组管理

- 创建用户组 groupadd 用户名
- 删除用户组 groupdel 用户名

#### 用户管理

- 创建用户 useradd [-g -d] 用户名
  - -g指定加入的用户组，没有则默认创建同名用户组
  - -d指定用户 HOME 路径
- 删除 userdel [-r] 用户名
  - -r 删除用户的 HOME 目录
- 查看用户所属组 id 用户名
  - 若不提供用户名 则默认查看自身
- 修改用户所属组 usermod -aG 用户组 用户名，将指定用户加入指定用户组

#### getent

- getent passwd 查看当前系统有哪些用户

  ![image-20230113145339392](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230113145339392.png)

- 共有7份信息，分别是：用户名:密码(x):用户ID:组ID:描述信息(无用):HOME目录:执行终端(默认bash)

  - getent group 查看当前系统中有哪些 用户组

    ![image-20230113145507808](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230113145507808.png)

- 包含3份信息，组名称:组认证(显示为x):组ID

### 权限控制

#### 认知权限信息

- ![image-20230113145813863](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230113145813863.png)
  - 从左到右依次为 文件权限控制信息、所属用户、所属用户组

- 权限槽位认识

  ![image-20230113150556736](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230113150556736.png)

- -:代表无此权限 r:读 w:写 x:执行

#### chmod 权限修改

- 可以利用 chmod 修改文件、文件夹的权限

  注意，只有文件、文件夹的所属用户或root用户可以修改。

- 语法：![image-20230113151407556](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230113151407556.png)

- -R 对文件夹内全部内容做相同操作

- 例如： chmod u=rwx,g=rx,o=x hello.txt 将文件权限修改为：rwxr-x--x

  - 其中：u表示user所属用户权限，g表示group组权限，o表示other其它用户权限

- chmod -R u=rwx,g=rx,o=x test，将文件夹test以及文件夹内全部内容权限设置为：rwxr-x--x

- 快捷写法：chmod 751 hello.txt

- 0：无任何权限，	即 ---1：仅有x权限，	即 --x2：仅有w权限	即 -w-3：有w和x权限	即 -wx4：仅有r权限	即 r--5：有r和x权限	即 r-x6：有r和w权限	即 rw-7：有全部权限	即 rwx

#### chown 修改文件、文件夹所属用户、组

- 语法：![image-20230113152530178](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230113152530178.png)
- 限制：只可 root 用户执行
- 示例：
  - chown root hello.txt，将hello.txt所属用户修改为root
  - chown :root hello.txt，将hello.txt所属用户组修改为root
  - chown root:itheima hello.txt，将hello.txt所属用户修改为root，用户组修改为itheima
  - chown -R root test，将文件夹test的所属用户修改为root并对文件夹内全部内容应用同样规则

### 快捷键

### 软件安装

#### yum 

- yum: RPM包软件管理器，用于自动化安装配置Linux软件，并可以自动解决依赖问题。
- 语法：![image-20230113163605866](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230113163605866.png)
- -y: 自动确认安装删除
- yum命令 需要在 root 用户下使用
- ubuntu 使用 apt 语法同 Linux

### systemctl

可以控制软件（服务）的启动、关闭、开机自启动

- 系统内置服务均可被systemctl控制
- 第三方软件，如果自动注册了可以被systemctl控制
- 第三方软件，如果没有自动注册，可以手动注册（后续学习）

- 语法：![image-20230113164657280](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230113164657280.png)

### ln 创建软连接

- 类似Windows系统中的《快捷方式》
- 语法：![image-20230114152214288](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230114152214288.png)
- 参数1：被链接的文件或文件夹 
- 参数2：要链接去的地址
- 实例：
  - ln -s /etc/yum.conf ~/yum.conf
  - ln -s /etc/yum ~/yum
  - ![image-20230114152505582](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230114152505582.png)

### date 

- 语法：![image-20230114153001883](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230114153001883.png)
- 修改 Linux时区
- ntp 自动联网同步时间

### IP地址 主机名

### 固定虚拟主机 IP

- 修改 ip 修改 网关
- 在 root 用户下，使用vim编辑/etc/sysconfig/network-scripts/ifcfg-ens33文件 
  - ![image-20230114163827318](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230114163827318.png)
- 重启网络 systemctl restart network 

### 网络传输

#### ping

- 语法：![image-20230114175749114](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230114175749114.png)
- -c，检查的次数，不使用-c选项，将无限次数持续检查
- ip或主机名，被检查的服务器的ip地址或主机名地址

#### wget

- 语法：![image-20230114175945268](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230114175945268.png)
- -b，可选，后台下载，会将日志写入到当前工作目录的wget-log文件
- url，下载链接

#### curl

- 语法：![image-20230115154057886](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230115154057886.png)
- 发起 http 网络请求 
- -0 下载文件时使用

### 查看端口占用

#### 查看对外暴露的端口

- 下载 nmap 
- 语法： nmap 被查看的 IP 地址

#### 查看某端口占用情况

- 下载 net-tools
- 语法：netstat -anp | grep 端口号

### 进程

#### 查看进程

- 语法：![image-20230115155942450](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230115155942450.png)
- 一般 ps -ef
- ![image-20230115160210268](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230115160210268.png)
- ![image-20230115160144067](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230115160144067.png)
- 查看指定程序 ps -ef | grep 内容
  - 内容可包括名称，进程号，用户ID

#### 关闭进程

- 语法：![image-20230115161633855](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230115161633855.png)
- -9： 强行关闭

### 主机状态

#### 查看资源占用

- 语法：top

#### top 命令内容详解

- 第一行：![image-20230115162327158](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230115162327158.png)

  top：命令名称，14:39:58：当前系统时间，up 6 min：启动了6分钟，2 users：2个用户登录，load：1、5、15分钟负载

- 第二行：![image-20230115162413815](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230115162413815.png)

  Tasks：175个进程，1 running：1个进程子在运行，174 sleeping：174个进程睡眠，0个停止进程，0个僵尸进程

- 第三行：![image-20230115162536229](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230115162536229.png)

  %Cpu(s)：CPU使用率，us：用户CPU使用率，sy：系统CPU使用率，ni：高优先级进程占用CPU时间百分比，id：空闲CPU率，wa：IO等待CPU占用率，hi：CPU硬件中断率，si：CPU软件中断率，st：强制等待占用CPU率

- 第四、五行![image-20230115162603959](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230115162603959.png)

  Kib Mem：物理内存，total：总量，free：空闲，used：使用，buff/cache：buff和cache占用KibSwap：虚拟内存（交换空间），total：总量，free：空闲，used：使用，buff/cache：buff和cache占用

- ![image-20230115163047018](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230115163047018.png)

  ![image-20230115163114287](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230115163114287.png)

- top 支持的选项

#### 磁盘信息监控

- df 命令查看磁盘的使用情况
- 语法：df -h
  - 选项：-h，以更加人性化的单位显示
- iostat 查看CPU、磁盘的相关信息
- 语法：iostat [-x] [num1] [num2]
  - 选项：-x，显示更多信息
  - num1：数字，刷新间隔，num2：数字，刷新几次
  - ![image-20230115163653721](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230115163653721.png)
  - tps：该设备每秒的传输次数（Indicate the number of transfers per second that were issued to the device.）。"一次传输"意思是"一次I/O请求"。多个逻辑请求可能会被合并为"一次I/O请求"。"一次传输"请求的大小是未知的。

- 使用iostat的-x选项，可以显示更多信息

  ![image-20230115164305544](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230115164305544.png)

#### 网络状态监控

- 语法：sar -n DEV num1 num2

  - -n，查看网络，DEV表示查看网络接口

  - num1：刷新间隔（不填就查看一次结束），num2：查看次数（不填无限次数）

  - ![image-20230115164721495](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230115164721495.png)

    

### 环境变量

- 环境变量讲解
- $ 符号 用于取变量的值

#### 自行设置环境变量

- 临时设置，语法：export 变量名=变量值

- 永久生效

  - 针对当前用户生效，配置在当前用户的：	~/.bashrc文件中

  - 针对所有用户生效，配置在系统的：	/etc/profile文件中

  - 并通过语法：source 配置文件，进行立刻生效，或重新登录FinalShell生效

  - 全局变量：

    ![image-20230115172313091](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230115172313091.png)

### 上传 下载

- 拖拽
- 下载 yum -y install lrzsz
- rz进行文件上传
- sz 文件，进行文件下载

### 压缩解压

#### tar 

- 语法：![image-20230116140146982](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230116140146982.png)
- ![image-20230116140210639](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230116140210639.png)
- ![image-20230116140325424](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230116140325424.png)
- ![image-20230116140416924](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230116140416924.png)

#### zip

- 语法：![image-20230116140611907](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230116140611907.png)
- -r，被压缩的包含文件夹的时候，需要使用-r选项，和rm、cp等命令的-r效果一致
- ![image-20230116140646520](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230116140646520.png)

#### unzip

- 语法：![image-20230116140740576](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230116140740576.png)
- ![image-20230116140812023](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230116140812023.png)
- ![image-20230116140822195](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230116140822195.png)

