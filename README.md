> [KMS的前世今生](https://github.com/Dwarven/vlmcsd/blob/doc/STORY.md)

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
CentOS 6

```
iptables -I INPUT -p tcp --dport 1688 -j ACCEPT
service iptables save
service iptables restart
```

CentOS 7

```
firewall-cmd --zone=public --add-port=1688/tcp --permanent
firewall-cmd --reload

or

systemctl disable firewalld
yum install iptables-services
systemctl enable iptables
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

如果不想用kms了，可以重置回原来的状态

```
slmgr.vbs -ckms  //清除系统KMS信息
slmgr.vbs -rearm //重置计算机的授权状态
```

### Windows GVLK密钥对照表（KMS激活专用）
kms激活的前提是你的系统是批量授权版本，即VL版，一般企业版都是VL版，专业版有零售和VL版，家庭版旗舰版OEM版等等那就肯定不能用kms激活。一般建议从[http://msdn.itellyou.cn](http://msdn.itellyou.cn/)上面下载系统 VL版本的镜像一般内置GVLK key，用于kms激活。如果你手动输过其他key，那么这个内置的key就会被替换掉，这个时候如果你想用kms，那么就需要把GVLK key输回去。首先， 到[https://technet.microsoft.com/zh-cn/library/jj612867.aspx](https://technet.microsoft.com/zh-cn/library/jj612867.aspx) 获取你对应版本的KEY 如果打不开下面有对应的

如果不知道自己的系统是什么版本，可以运行以下命令查看系统版本：

```
wmic os get caption
```
> 以下key来源于微软官网：[https://technet.microsoft.com/zh-cn/library/jj612867.aspx](https://technet.microsoft.com/zh-cn/library/jj612867.aspx)

#### Windows 10

| 操作系统 | KMS激活序列号 |
| --- | --- |
| Windows 10 Home | TX9XD-98N7V-6WMQ6-BX7FG-H8Q99 |
| Windows 10 Home N | 3KHY7-WNT83-DGQKR-F7HPR-844BM |
| Windows 10 Home Single Language | 7HNRX-D7KGG-3K4RQ-4WPJ4-YTDFH |
| Windows 10 Home Country Specific | PVMJN-6DFY6-9CCP6-7BKTT-D3WVR |
| Windows 10 Professional | W269N-WFGWX-YVC9B-4J6C9-T83GX |
| Windows 10 Professional N | MH37W-N47XK-V7XM9-C7227-GCQG9 |
| Windows 10 Professional Education | 6TP4R-GNPTD-KYYHQ-7B7DP-J447Y |
| Windows 10 Professional Education N | YVWGF-BXNMC-HTQYQ-CPQ99-66QFC |
| Windows 10 Professional Workstation | NRG8B-VKK3Q-CXVCJ-9G2XF-6Q84J |
| Windows 10 Professional Workstation N | 9FNHH-K3HBT-3W4TD-6383H-6XYWF |
| Windows 10 Education | NW6C2-QMPVW-D7KKK-3GKT6-VCFB2 |
| Windows 10 Education N | 2WH4N-8QGBV-H22JP-CT43Q-MDWWJ |
| Windows 10 Enterprise | NPPR9-FWDCX-D2C8J-H872K-2YT43 |
| Windows 10 Enterprise N | DPH2V-TTNVB-4X9Q3-TJR4H-KHJW4 |
| Windows 10 Enterprise G | YYVX9-NTFWV-6MDM3-9PT4T-4M68B |
| Windows 10 Enterprise G N | 44RPN-FTY23-9VTTB-MP9BX-T84FV |
| Windows 10 Enterprise 2015 LTSB | WNMTR-4C88C-JK8YV-HQ7T2-76DF9 |
| Windows 10 Enterprise 2015 LTSB N | 2F77B-TNFGY-69QQF-B8YKP-D69TJ |
| Windows 10 Enterprise 2016 LTSB | DCPHK-NFMTC-H88MJ-PFHPY-QJ4BJ |
| Windows 10 Enterprise 2016 LTSB N | QFFDN-GRT3P-VKWWX-X7T3R-8B639 |
| Windows 10 Enterprise LTSC 2018 | M7XTQ-FN8P6-TTKYV-9D4CC-J462D |
| Windows 10 Enterprise LTSC 2018 N | 92NFX-8DJQP-P6BBQ-THF9C-7CG2H |
| Windows 10 Enterprise Remote Server | 7NBT4-WGBQX-MP4H7-QXFF8-YP3KX |
| Windows 10 Enterprise for Remote Sessions | CPWHC-NT2C7-VYW78-DHDB2-PG3GK |
| Windows 10 Lean | NBTWJ-3DR69-3C4V8-C26MC-GQ9M6 |

#### Windows Server 2019

| 操作系统 | KMS激活序列号 |
| --- | --- |
| Windows Server 2019 Essentials | WVDHN-86M7X-466P6-VHXV7-YY726 |
| Windows Server 2019 Standard | N69G4-B89J2-4G8F4-WWYCC-J464C |
| Windows Server 2019 Datacenter | WMDGN-G9PQG-XVVXX-R3X43-63DFG |
| Windows Server 2019 Standard ACor | N2KJX-J94YW-TQVFB-DG9YT-724CC |
| Windows Server 2019 Datacenter ACor | 6NMRW-2C8FM-D24W7-TQWMY-CWH2D |
| Windows Server 2019 Azure Core | FDNH6-VW9RW-BXPJ7-4XTYG-239TB |
| Windows Server 2019 ARM64 | GRFBW-QNDC4-6QBHG-CCK3B-2PR88 |

#### Windows Server 2016

| 操作系统 | KMS激活序列号 |
| --- | --- |
| Windows Server 2016 Datacenter | CB7KF-BWN84-R7R2Y-793K2-8XDDG |
| Windows Server 2016 Standard | WC2BQ-8NRM3-FDDYY-2BFGV-KHKQY |
| Windows Server 2016 Essentials | JCKRF-N37P4-C2D82-9YXRT-4M63B |
| Windows Server 2016 Standard ACor | PTXN8-JFHJM-4WC78-MPCBR-9W4KR |
| Windows Server 2016 Datacenter ACor | 2HXDN-KRXHB-GPYC7-YCKFJ-7FVDG |
| Windows Server 2016 Cloud Storage | QN4C6-GBJD2-FB422-GHWJK-GJG2R |
| Windows Server 2016 Azure Core | VP34G-4NPPG-79JTQ-864T4-R3MQX |
| Windows Server 2016 ARM64 | K9FYF-G6NCK-73M32-XMVPY-F9DRR |

#### Windows 8.1

| 操作系统 | KMS激活序列号 |
| --- | --- |
| Windows 8.1 Professional | GCRJD-8NW9H-F2CDX-CCM8D-9D6T9 |
| Windows 8.1 Professional N | HMCNV-VVBFX-7HMBH-CTY9B-B4FXY |
| Windows 8.1 Enterprise | MHF9N-XY6XB-WVXMC-BTDCT-MKKG7 |
| Windows 8.1 Enterprise N | TT4HM-HN7YT-62K67-RGRQJ-JFFXW |
| Windows 8.1 Professional WMC | 789NJ-TQK6T-6XTH8-J39CJ-J8D3P |
| Windows 8.1 Core | M9Q9P-WNJJT-6PXPY-DWX8H-6XWKK |
| Windows 8.1 Core N | 7B9N3-D94CG-YTVHR-QBPX3-RJP64 |
| Windows 8.1 Core ARM | XYTND-K6QKT-K2MRH-66RTM-43JKP |
| Windows 8.1 Core Single Language | BB6NG-PQ82V-VRDPW-8XVD2-V8P66 |
| Windows 8.1 Core Country Specific | NCTT7-2RGK8-WMHRF-RY7YQ-JTXG3 |
| Windows 8.1 Embedded Industry | NMMPB-38DD4-R2823-62W8D-VXKJB |
| Windows 8.1 Embedded Industry Enterprise | FNFKF-PWTVT-9RC8H-32HB2-JB34X |
| Windows 8.1 Embedded Industry Automotive | VHXM3-NR6FT-RY6RT-CK882-KW2CJ |
| Windows 8.1 Core Connected (with Bing) | 3PY8R-QHNP9-W7XQD-G6DPH-3J2C9 |
| Windows 8.1 Core Connected N (with Bing) | Q6HTR-N24GM-PMJFP-69CD8-2GXKR |
| Windows 8.1 Core Connected Single Language (with Bing) | KF37N-VDV38-GRRTV-XH8X6-6F3BB |
| Windows 8.1 Core Connected Country Specific (with Bing) | R962J-37N87-9VVK2-WJ74P-XTMHR |
| Windows 8.1 Professional Student | MX3RK-9HNGX-K3QKC-6PJ3F-W8D7B |
| Windows 8.1 Professional Student N | TNFGH-2R6PB-8XM3K-QYHX2-J4296 |

#### Windows Server 2012 R2

| 操作系统 | KMS激活序列号 |
| --- | --- |
| Windows Server 2012 R2 Standard | D2N9P-3P6X9-2R39C-7RTCD-MDVJX |
| Windows Server 2012 R2 Datacenter | W3GGN-FT8W3-Y4M27-J84CP-Q3VJ9 |
| Windows Server 2012 R2 Essentials | KNC87-3J2TX-XB4WP-VCPJV-M4FWM |
| Windows Server 2012 R2 Cloud Storage | 3NPTF-33KPT-GGBPR-YX76B-39KDD |

#### Windows 8

| 操作系统 | KMS激活序列号 |
| --- | --- |
| Windows 8 Professional | NG4HW-VH26C-733KW-K6F98-J8CK4 |
| Windows 8 Professional N | XCVCF-2NXM9-723PB-MHCB7-2RYQQ |
| Windows 8 Enterprise | 32JNW-9KQ84-P47T8-D8GGY-CWCK7 |
| Windows 8 Enterprise N | JMNMF-RHW7P-DMY6X-RF3DR-X2BQT |
| Windows 8 Professional WMC | GNBB8-YVD74-QJHX6-27H4K-8QHDG |
| Windows 8 Core | BN3D2-R7TKB-3YPBD-8DRP2-27GG4 |
| Windows 8 Core N | 8N2M2-HWPGY-7PGT9-HGDD8-GVGGY |
| Windows 8 Core Single Language | 2WN2H-YGCQR-KFX6K-CD6TF-84YXQ |
| Windows 8 Core Country Specific | 4K36P-JN4VD-GDC6V-KDT89-DYFKP |
| Windows 8 Core ARM | DXHJF-N9KQX-MFPVR-GHGQK-Y7RKV |
| Windows 8 Embedded Industry Professional | RYXVT-BNQG7-VD29F-DBMRY-HT73M |
| Windows 8 Embedded Industry Enterprise | NKB3R-R2F8T-3XCDP-7Q2KW-XWYQ2 |

#### Windows Server 2012

| 操作系统 | KMS激活序列号 |
| --- | --- |
| Windows Server 2012 Standard | XC9B7-NBPP2-83J2H-RHMBY-92BT4 |
| Windows Server 2012 Datacenter | 48HP8-DN98B-MYWDG-T2DCC-8W83P |
| Windows Server 2012 MultiPoint Standard | HM7DN-YVMH3-46JC3-XYTG7-CYQJJ |
| Windows Server 2012 MultiPoint Premium | XNH6W-2V9GX-RGJ4K-Y8X6F-QGJ2G |

#### Windows 7

| 操作系统 | KMS激活序列号 |
| --- | --- |
| Windows 7 Professional | FJ82H-XT6CR-J8D7P-XQJJ2-GPDD4 |
| Windows 7 Professional N | MRPKT-YTG23-K7D7T-X2JMM-QY7MG |
| Windows 7 Professional E | W82YF-2Q76Y-63HXB-FGJG9-GF7QX |
| Windows 7 Enterprise | 33PXH-7Y6KF-2VJC9-XBBR8-HVTHH |
| Windows 7 Enterprise N | YDRBP-3D83W-TY26F-D46B2-XCKRJ |
| Windows 7 Enterprise E | C29WB-22CC8-VJ326-GHFJW-H9DH4 |
| Windows 7 Embedded POS Ready | YBYF6-BHCR3-JPKRB-CDW7B-F9BK4 |
| Windows 7 Embedded ThinPC | 73KQT-CD9G6-K7TQG-66MRP-CQ22C |
| Windows 7 Embedded Standard | XGY72-BRBBT-FF8MH-2GG8H-W7KCW |


#### Windows Server 2008 R2

| 操作系统 | KMS激活序列号 |
| --- | --- |
| Windows Server 2008 R2 Web | 6TPJF-RBVHG-WBW2R-86QPH-6RTM4 |
| Windows Server 2008 R2 HPC edition | TT8MH-CG224-D3D7Q-498W2-9QCTX |
| Windows Server 2008 R2 Standard | YC6KT-GKW9T-YTKYR-T4X34-R7VHC |
| Windows Server 2008 R2 Enterprise | 489J6-VHDMP-X63PK-3K798-CPX3Y |
| Windows Server 2008 R2 Datacenter | 74YFP-3QFB3-KQT8W-PMXWJ-7M648 |
| Windows Server 2008 R2 for Itanium-based Systems | GT63C-RJFQ3-4GMB6-BRFB9-CB83V |
| Windows MultiPoint Server 2010 | 736RG-XDKJK-V34PF-BHK87-J6X3K |

#### Windows Vista

| 操作系统 | KMS激活序列号 |
| --- | --- |
| Windows Vista Business | YFKBB-PQJJV-G996G-VWGXY-2V3X8 |
| Windows Vista Business N | HMBQG-8H2RH-C77VX-27R82-VMQBT |
| Windows Vista Enterprise | VKK3X-68KWM-X2YGT-QR4M6-4BWMV |
| Windows Vista Enterprise N | VTC42-BM838-43QHV-84HX6-XJXKV |

#### Windows Server 2008

| 操作系统 | KMS激活序列号 |
| --- | --- |
| Windows Server 2008 Web | WYR28-R7TFJ-3X2YQ-YCY4H-M249D |
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

### Office GVLK密钥对照表（KMS激活专用）

> 以下key来源于微软官网：
> [Office 2010](https://technet.microsoft.com/zh-cn/library/ee624355(office.14).aspx)
> [Office 2013](https://technet.microsoft.com/zh-cn/library/dn385360.aspx)
> [Office 2016](https://technet.microsoft.com/zh-cn/library/dn385360(v=office.16).aspx)



#### Office 2010

| Product | GVLK |
| --- | --- |
| Office Professional Plus 2010 | VYBBJ-TRJPB-QFQRF-QFT4D-H3GVB |
| Office Standard 2010 | V7QKV-4XVVR-XYV4D-F7DFM-8R6BM |
| Access 2010 | V7Y44-9T38C-R2VJK-666HK-T7DDX |
| Excel 2010 | H62QG-HXVKF-PP4HP-66KMR-CW9BM |
| SharePoint Workspace 2010 | QYYW6-QP4CB-MBV6G-HYMCJ-4T3J4 |
| InfoPath 2010 | K96W8-67RPQ-62T9Y-J8FQJ-BT37T |
| OneNote 2010 | Q4Y4M-RHWJM-PY37F-MTKWH-D3XHX |
| Outlook 2010 | 7YDC2-CWM8M-RRTJC-8MDVC-X3DWQ |
| PowerPoint 2010 | RC8FX-88JRY-3PF7C-X8P67-P4VTT |
| Project Professional 2010 | YGX6F-PGV49-PGW3J-9BTGG-VHKC6 |
| Project Standard 2010 | 4HP3K-88W3F-W2K3D-6677X-F9PGB |
| Publisher 2010 | BFK7F-9MYHM-V68C7-DRQ66-83YTP |
| Word 2010 | HVHB3-C6FV7-KQX9W-YQG79-CRY7T |
| Visio Premium 2010 | D9DWC-HPYVV-JGF4P-BTWQB-WX8BJ |
| Visio Professional 2010 | 7MCW8-VRQVK-G677T-PDJCM-Q8TCP |
| Visio Standard 2010 | 767HD-QGMWX-8QTDB-9G3R2-KHFGJ |

#### Office 2013

| Product | GVLK |
| --- | --- |
| Office 2013 Professional Plus | YC7DK-G2NP3-2QQC3-J6H88-GVGXT |
| Office 2013 Standard | KBKQT-2NMXY-JJWGP-M62JB-92CD4 |
| Project 2013 Professional | FN8TT-7WMH6-2D4X9-M337T-2342K |
| Project 2013 Standard | 6NTH3-CW976-3G3Y2-JK3TX-8QHTT |
| Visio 2013 Professional | C2FG9-N6J68-H8BTJ-BW3QX-RM3B3 |
| Visio 2013 Standard | J484Y-4NKBF-W2HMG-DBMJC-PGWR7 |
| Access 2013 | NG2JY-H4JBT-HQXYP-78QH9-4JM2D |
| Excel 2013 | VGPNG-Y7HQW-9RHP7-TKPV3-BG7GB |
| InfoPath 2013 | DKT8B-N7VXH-D963P-Q4PHY-F8894 |
| Lync 2013 | 2MG3G-3BNTT-3MFW9-KDQW3-TCK7R |
| OneNote 2013 | TGN6P-8MMBC-37P2F-XHXXK-P34VW |
| Outlook 2013 | QPN8Q-BJBTJ-334K3-93TGY-2PMBT |
| PowerPoint 2013 | 4NT99-8RJFH-Q2VDH-KYG2C-4RD4F |
| Publisher 2013 | PN2WF-29XG2-T9HJ7-JQPJR-FCXK4 |
| Word 2013 | 6Q7VD-NX8JD-WJ2VH-88V73-4GBJ7 |

#### Office 2016

| Product | GVLK |
| --- | --- |
| Office Professional Plus 2016 | XQNVK-8JYDB-WJ9W3-YJ8YR-WFG99 |
| Office Standard 2016 | JNRGM-WHDWX-FJJG3-K47QV-DRTFM |
| Project Professional 2016 | YG9NW-3K39V-2T3HJ-93F3Q-G83KT |
| Project Standard 2016 | GNFHQ-F6YQM-KQDGJ-327XX-KQBVC |
| Visio Professional 2016 | PD3PC-RHNGV-FXJ29-8JK7D-RJRJK |
| Visio Standard 2016 | 7WHWN-4T7MP-G96JF-G33KR-W8GF4 |
| Access 2016 | GNH9Y-D2J4T-FJHGG-QRVH7-QPFDW |
| Excel 2016 | 9C2PK-NWTVB-JMPW8-BFT28-7FTBF |
| OneNote 2016 | DR92N-9HTF2-97XKM-XW2WJ-XW3J6 |
| Outlook 2016 | R69KK-NTPKF-7M3Q4-QYBHW-6MT9B |
| PowerPoint 2016 | J7MQP-HNJ4Y-WJ7YM-PFYGF-BY6C6 |
| Publisher 2016 | F47MM-N3XJP-TQXJ9-BP99D-8K837 |
| Skype for Business 2016 | 869NQ-FJ69K-466HW-QYCP2-DDBV6 |
| Word 2016 | WXY84-JN2Q9-RBCCQ-3Q3J3-3PFJ6 |

#### Office 2019

| Product | GVLK |
| --- | --- |
| Office 2019 Professional Plus | NMMKJ-6RK4F-KMJVX-8D9MJ-6MWKP |
| Office 2019 Standard | 6NWWJ-YQWMR-QKGCB-6TMB3-9D9HK |
| Project 2019 Professional | B4NPR-3FKK7-T2MBV-FRQ4W-PKD2B |
| Project 2019 Standard | C4F7P-NCP8C-6CQPT-MQHV9-JXD2M |
| Visio 2019 Professional | 9BGNQ-K37YR-RQHF2-38RQ3-7VCBB |
| Visio 2019 Standard | 7TQNQ-K3YQQ-3PFH7-CCPPM-X4VQ2 |
| Access 2019 | 9N9PT-27V4Y-VJ2PD-YXFMF-YTFQT |
| Excel 2019 | TMJWT-YYNMB-3BKTF-644FC-RVXBD |
| Outlook 2019 | 7HD7K-N4PVK-BHBCQ-YWQRW-XW4VK |
| PowerPoint 2019 | RRNCX-C64HY-W2MM7-MCH9G-TJHMQ |
| Publisher 2019 | G2KWX-3NW6P-PY93R-JXK2T-C9Y9V |
| Skype for Business 2019 | NCJ33-JHBBY-HTK98-MYCV8-HMKHJ |
| Word 2019 | PBX3G-NWMT6-Q7XBW-PYJGG-WXD33 |
| Office 2019 Professional Plus C2R-P | VQ9DP-NVHPH-T9HJC-J9PDT-KTQRG |
| Project 2019 Professional C2R-P | XM2V9-DN9HH-QB449-XDGKC-W2RMW |
| Visio 2019 Professional C2R-P | N2CG9-YD3YK-936X4-3WR82-Q3X4H |

# 附
[KMS VL ALL](https://forums.mydigitallife.net/threads/kms_vl_all-online-offline-kms-activator.63471/)

