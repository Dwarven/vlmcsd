> [KMS的前世今生](https://github.com/Dwarven/vlmcsd/blob/archive/STORY.md)

# 1.KMS服务器搭建
通过Linux服务器搭建Windows KMS激活服务器，可以用来激活Windows和Office ，下面开始教程

> 可以从[MDL](https://forums.mydigitallife.net/threads/emulated-kms-servers-on-non-windows-platforms.50234/)下载最新版的VLMCSD (Source and binaries)，然后找到自己选择平台对应的二进制文件
> 这里以`CentOS`为例

### 永久关闭SELinux

```
sed -i 's/^SELINUX=.*/#&/;s/^SELINUXTYPE=.*/#&/;/SELINUX=.*/a SELINUX=disabled' /etc/sysconfig/selinux && /usr/sbin/setenforce 0
```

### 下载 vlmcsd

x64

```
wget https://raw.githubusercontent.com/Dwarven/vlmcsd/master/binaries/Linux/intel/static/vlmcsd-x64-musl-static -O KMS-server
```

x86

```
wget https://raw.githubusercontent.com/Dwarven/vlmcsd/master/binaries/Linux/intel/static/vlmcsd-x86-musl-static -O KMS-server
```

### 创建快捷方式

```
ln -sv KMS-server /usr/local/KMS-server
```

### 赋予权限

```
chmod u+x /usr/local/KMS-server
```

### 执行程序

```
/usr/local/KMS-server
```

### 防火墙设置

```
iptables -I INPUT -p tcp --dport 1688 -j ACCEPT
service iptables save
service iptables restart
```

### 开机启动

```
echo "/usr/local/KMS-server" >> /etc/rc.d/rc.local
```

# 2.Windows 激活方法

```
 slmgr.vbs -upk
 slmgr.vbs -ipk XXXXX-XXXXX-XXXXX-XXXXX-XXXXX //填入自己Windows版本对应的密钥
 slmgr.vbs -skms 192.168.1.1 //这个IP地址是你KMS服务器的地址
 slmgr.vbs -ato
 slmgr.vbs -dlv
```

### Windows GVLK密钥对照表（KMS激活专用）
kms激活的前提是你的系统是批量授权版本，即VL版，一般企业版都是VL版，专业版有零售和VL版，家庭版旗舰版OEM版等等那就肯定不能用kms激活。一般建议从[http://msdn.itellyou.cn](http://msdn.itellyou.cn/)上面下载系统 VL版本的镜像一般内置GVLK key，用于kms激活。如果你手动输过其他key，那么这个内置的key就会被替换掉，这个时候如果你想用kms，那么就需要把GVLK key输回去。首先， 到[https://technet.microsoft.com/en-us/library/jj612867.aspx](https://technet.microsoft.com/en-us/library/jj612867.aspx) 获取你对应版本的KEY 如果打不开下面有对应的

如果不知道自己的系统是什么版本，可以运行以下命令查看系统版本：

```
wmic os get caption
```
> 以下key来源于微软官网：[https://technet.microsoft.com/en-us/library/jj612867.aspx](https://technet.microsoft.com/en-us/library/jj612867.aspx)

#### Windows Server 2016

| 操作系统 | KMS激活序列号 |
| --- | --- |
| Windows Server 2016 Datacenter | CB7KF-BWN84-R7R2Y-793K2-8XDDG |
| Windows Server 2016 Standard | WC2BQ-8NRM3-FDDYY-2BFGV-KHKQY |
| Windows Server 2016 Essentials | JCKRF-N37P4-C2D82-9YXRT-4M63B |

#### Windows 10

| 操作系统 | KMS激活序列号 |
| --- | --- |
| Windows 10 Professional | W269N-WFGWX-YVC9B-4J6C9-T83GX |
| Windows 10 Professional N | MH37W-N47XK-V7XM9-C7227-GCQG9 |
| Windows 10 Enterprise | NPPR9-FWDCX-D2C8J-H872K-2YT43 |
| Windows 10 Enterprise N | DPH2V-TTNVB-4X9Q3-TJR4H-KHJW4 |
| Windows 10 Education | NW6C2-QMPVW-D7KKK-3GKT6-VCFB2 |
| Windows 10 Education N | 2WH4N-8QGBV-H22JP-CT43Q-MDWWJ |
| Windows 10 Enterprise 2015 LTSB | WNMTR-4C88C-JK8YV-HQ7T2-76DF9 |
| Windows 10 Enterprise 2015 LTSB N | 2F77B-TNFGY-69QQF-B8YKP-D69TJ |
| Windows 10 Enterprise 2016 LTSB | DCPHK-NFMTC-H88MJ-PFHPY-QJ4BJ |
| Windows 10 Enterprise 2016 LTSB N | QFFDN-GRT3P-VKWWX-X7T3R-8B639 |

#### Windows Server 2012 R2 和 Windows 8.1

| 操作系统 | KMS激活序列号 |
| --- | --- |
| Windows 8.1 Professional | GCRJD-8NW9H-F2CDX-CCM8D-9D6T9 |
| Windows 8.1 Professional N | HMCNV-VVBFX-7HMBH-CTY9B-B4FXY |
| Windows 8.1 Enterprise | MHF9N-XY6XB-WVXMC-BTDCT-MKKG7 |
| Windows 8.1 Enterprise N | TT4HM-HN7YT-62K67-RGRQJ-JFFXW |
| Windows Server 2012 R2 Server Standard | D2N9P-3P6X9-2R39C-7RTCD-MDVJX |
| Windows Server 2012 R2 Datacenter | W3GGN-FT8W3-Y4M27-J84CP-Q3VJ9 |
| Windows Server 2012 R2 Essentials | KNC87-3J2TX-XB4WP-VCPJV-M4FWM |

#### Windows Server 2012 和 Windows 8

| 操作系统 | KMS激活序列号 |
| --- | --- |
| Windows 8 Professional | NG4HW-VH26C-733KW-K6F98-J8CK4 |
| Windows 8 Professional N | XCVCF-2NXM9-723PB-MHCB7-2RYQQ |
| Windows 8 Enterprise | 32JNW-9KQ84-P47T8-D8GGY-CWCK7 |
| Windows 8 Enterprise N | JMNMF-RHW7P-DMY6X-RF3DR-X2BQT |
| Windows Server 2012 | BN3D2-R7TKB-3YPBD-8DRP2-27GG4 |
| Windows Server 2012 N | 8N2M2-HWPGY-7PGT9-HGDD8-GVGGY |
| Windows Server 2012 Single Language | 2WN2H-YGCQR-KFX6K-CD6TF-84YXQ |
| Windows Server 2012 Country Specific | 4K36P-JN4VD-GDC6V-KDT89-DYFKP |
| Windows Server 2012 Server Standard | XC9B7-NBPP2-83J2H-RHMBY-92BT4 |
| Windows Server 2012 MultiPoint Standard | HM7DN-YVMH3-46JC3-XYTG7-CYQJJ |
| Windows Server 2012 MultiPoint Premium | XNH6W-2V9GX-RGJ4K-Y8X6F-QGJ2G |
| Windows Server 2012 Datacenter | 48HP8-DN98B-MYWDG-T2DCC-8W83P |

#### Windows 7 and Windows Server 2008 R2

| 操作系统 | KMS激活序列号 |
| --- | --- |
| Windows 7 Professional | FJ82H-XT6CR-J8D7P-XQJJ2-GPDD4 |
| Windows 7 Professional N | MRPKT-YTG23-K7D7T-X2JMM-QY7MG |
| Windows 7 Professional E | W82YF-2Q76Y-63HXB-FGJG9-GF7QX |
| Windows 7 Enterprise | 33PXH-7Y6KF-2VJC9-XBBR8-HVTHH |
| Windows 7 Enterprise N | YDRBP-3D83W-TY26F-D46B2-XCKRJ |
| Windows 7 Enterprise E | C29WB-22CC8-VJ326-GHFJW-H9DH4 |
| Windows Server 2008 R2 Web | 6TPJF-RBVHG-WBW2R-86QPH-6RTM4 |
| Windows Server 2008 R2 HPC edition | TT8MH-CG224-D3D7Q-498W2-9QCTX |
| Windows Server 2008 R2 Standard | YC6KT-GKW9T-YTKYR-T4X34-R7VHC |
| Windows Server 2008 R2 Enterprise | 489J6-VHDMP-X63PK-3K798-CPX3Y |
| Windows Server 2008 R2 Datacenter | 74YFP-3QFB3-KQT8W-PMXWJ-7M648 |
| Windows Server 2008 R2 for Itanium-based Systems | GT63C-RJFQ3-4GMB6-BRFB9-CB83V |

#### Windows Vista and Windows Server 2008

| 操作系统 | KMS激活序列号 |
| --- | --- |
| Windows Vista Business | YFKBB-PQJJV-G996G-VWGXY-2V3X8 |
| Windows Vista Business N | HMBQG-8H2RH-C77VX-27R82-VMQBT |
| Windows Vista Enterprise | VKK3X-68KWM-X2YGT-QR4M6-4BWMV |
| Windows Vista Enterprise N | VTC42-BM838-43QHV-84HX6-XJXKV |
| Windows Web Server 2008 | WYR28-R7TFJ-3X2YQ-YCY4H-M249D |
| Windows Server 2008 Standard | TM24T-X9RMF-VWXK6-X8JC9-BFGM2 |
| Windows Server 2008 Standard without Hyper-V | W7VD6-7JFBR-RX26B-YKQ3Y-6FFFJ |
| Windows Server 2008 Enterprise | YQGMW-MPWTJ-34KDK-48M3W-X4Q6V |
| Windows Server 2008 Enterprise without Hyper-V | 39BXF-X8Q23-P2WWT-38T2F-G3FPG |
| Windows Server 2008 HPC | RCTX3-KWVHP-BR6TB-RB6DM-6X7HP |
| Windows Server 2008 Datacenter | 7M67G-PC374-GR742-YH8V4-TCBY3 |
| Windows Server 2008 Datacenter without Hyper-V | 22XQ2-VRXRG-P8D42-K34TD-G3QQC |
| Windows Server 2008 for Itanium-Based Systems | 4DWFP-JF3DJ-B7DTH-78FJB-PDRHK |

# 3.一句命令激活OFFICE

首先你的OFFICE必须是VOL版本，否则无法激活。 找到你的office安装目录，比如

```
C:\Program Files (x86)\Microsoft Office\Office16
```

64位的就是

```
C:\Program Files\Microsoft Office\Office16
```

office16是office2016，office15就是2013，office14就是2010.

然后目录对的话，该目录下面应该有个OSPP.VBS。

接下来我们就cd到这个目录下面，例如：

```
cd C:\Program Files (x86)\Microsoft Office\Office16
```

然后执行注册kms服务器地址，注意office不支持  address:port 这种写法：

```
// cscript ospp.vbs /setprt:1688
cscript ospp.vbs /sethst:192.168.1.1 //这个IP地址是你KMS服务器的地址
```

/sethst参数就是指定kms服务器地址。

一般ospp.vbs可以拖进去cmd窗口，所以也可以这么弄：

```
cscript "C:\Program Files (x86)\Microsoft Office\Office16\OSPP.VBS" /sethst:192.168.1.1 //这个IP地址是你KMS服务器的地址
```

一般来说，“一句命令已经完成了”，但一般office不会马上连接kms服务器进行激活，所以我们额外补充一条手动激活命令：

```
cscript ospp.vbs /act
```

如果提示看到successful的字样，那么就是激活成功了，重新打开office就好。

检查激活状态：

```
cscript ospp.vbs /dstatus
```

最后一条命令执行完成之后，看到LICENSE STATUS:  ---LICENSED---字样，说明激活成功。

#### 如果遇到报错，请检查：

> 1. 你的系统/OFFICE是否是批量VL版本
> 2. 是否以管理员权限运行CMD
> 3. 你的系统/OFFICE是否修改过KEY/未安装GVLK KEY
> 4. 检查你的网络连接
> 5. 服务器繁忙，多试试（点击检查KMS服务是否可用）
> 6. 根据出错代码自己搜索出错原因


