# 无线安全研究全景知识树

> 覆盖 WiFi / 蓝牙 / Sub-GHz / 无人机 / 嵌入式实现 · 知识点总计 230+
> 学习定位：防御性研究 · 授权渗透测试 · 安全设备开发

---

## 第一章：射频与物理层基础

### 1.1 电磁波基础

- **KP-01** 波长 · 频率 · 振幅三角关系（λ = c / f）
- **KP-02** 电磁波传播模式：自由空间直射、多径反射、衍射、散射
- **KP-03** 多径效应与多径衰落（Multipath Fading）
- **KP-04** 天线增益单位：dBi（相对各向同性天线）vs dBd（相对偶极子）
- **KP-05** EIRP（等效各向同性辐射功率）计算：EIRP = 发射功率(dBm) + 天线增益(dBi) - 线缆损耗
- **KP-06** Friis 自由空间传播方程与路径损耗模型
- **KP-07** 菲涅耳区概念与障碍物对信号的影响
- **KP-08** 反射、折射、衍射对室内覆盖的工程影响

### 1.2 WiFi 频段

- **KP-09** 2.4 GHz ISM 频段：信道 1–13（中国）/ 1–11（美国），20/40 MHz 宽度
- **KP-10** 2.4 GHz 非重叠信道：1、6、11（三信道规划），信道间隔 5 MHz
- **KP-11** 5 GHz U-NII 频段：U-NII-1/2/2e/3，信道 36–165
- **KP-12** DFS（动态频率选择）：5 GHz 雷达保护信道的自动切换机制
- **KP-13** 6 GHz 频段（WiFi 6E）：U-NII-5/6/7/8，59 个非重叠 20 MHz 信道
- **KP-14** 60 GHz 频段（802.11ad/ay WiGig）：极高吞吐量 + 极短传播距离

### 1.3 调制编码技术

- **KP-15** DSSS（直接序列扩频）：802.11b 使用，11 Mbps 上限，抗干扰强
- **KP-16** OFDM（正交频分复用）：802.11a/g/n/ac 核心，52 子载波分割频带
- **KP-17** OFDMA（正交频分多址）：802.11ax（WiFi 6）多用户并发，RU（资源单元）分配
- **KP-18** MU-MIMO（多用户多输入多输出）：下行/上行多用户并发传输
- **KP-19** 空间流（Spatial Streams）：MIMO 的核心概念，增加吞吐量而非覆盖
- **KP-20** MCS（调制编码方案）索引：从 BPSK 1/2 到 256-QAM 5/6
- **KP-21** FHSS（跳频扩频）：早期蓝牙核心机制，伪随机跳频序列
- **KP-22** BSS Coloring（BSS 着色）：WiFi 6 的同频干扰识别与规避机制

### 1.4 信号质量指标

- **KP-23** RSSI（接收信号强度指示）：单位 dBm，典型范围 -30（极强）到 -90（弱）
- **KP-24** SNR（信噪比）：信号强度 - 噪声本底，影响最高 MCS 选择
- **KP-25** 噪声本底（Noise Floor）：-95 dBm 典型值，受环境温度和带宽影响
- **KP-26** 链路预算计算：发射功率 + 天线增益 - 路径损耗 - 接收灵敏度 = 余量
- **KP-27** 重传率（Retry Rate）：反映无线链路质量的关键指标
- **KP-28** 信道利用率（Channel Utilization）：无线介质忙碌时间占比

### 1.5 信道干扰

- **KP-29** 同频干扰（CCI，Co-Channel Interference）：同信道竞争，降低整体吞吐
- **KP-30** 邻频干扰（ACI，Adjacent Channel Interference）：非正交信道溢出能量
- **KP-31** 隐藏节点问题（Hidden Node Problem）与 RTS/CTS 握手解决机制
- **KP-32** 曝露节点问题（Exposed Node Problem）
- **KP-33** 微波炉（~2.45 GHz）/ 蓝牙 / ZigBee / 婴儿监护器对 2.4 GHz 的干扰特征

---

## 第二章：802.11 协议栈深度

### 2.1 帧类型总体架构

- **KP-34** 三类帧：管理帧（Type 00）/ 控制帧（Type 01）/ 数据帧（Type 10）
- **KP-35** 帧控制字段（Frame Control）：Protocol Version / Type / Subtype / Flags
- **KP-36** To DS / From DS 标志：确定四地址字段中 SA/DA/BSSID/RA 的对应关系
- **KP-37** Duration/ID 字段：NAV（网络分配矢量）机制，虚拟信道占用

### 2.2 管理帧（GhostESP 操作核心）

- **KP-38** Beacon 帧：AP 周期性广播（默认 102.4 ms），包含 SSID / BSSID / 能力集 / 支持速率 / IE 列表
- **KP-39** 信息元素（IE，Information Element）体系：TLV 编码，可扩展，如 RSN IE / HT IE / VHT IE / HE IE
- **KP-40** Probe Request：STA 主动探测，携带期望 SSID（或广播），暴露已知网络列表（PNL）
- **KP-41** Probe Response：AP 对 Probe Request 的精确回复，内容等同 Beacon
- **KP-42** Authentication 帧（Open System）：两帧握手完成第一层认证（State 1→State 2）
- **KP-43** Association Request / Response：STA 选择 AP 后的绑定过程（State 2→State 3）
- **KP-44** Reassociation Request / Response：漫游到新 AP 时保留上下文的关联重建
- **KP-45** Deauthentication 帧：单向通知，Reason Code 字段（1=Unspecified / 3=STA Leaving / 7=Class 3 frame received），**不受 PMF 保护时无需认证**
- **KP-46** Disassociation 帧：解除关联但保留认证状态（State 3→State 2）
- **KP-47** Action 帧：承载高级功能，如 CSA（信道切换通知）/ RRM（无线资源管理）/ FT（快速漫游）
- **KP-48** ATIM 帧：IBSS（Ad-Hoc）模式下的节电传输公告

### 2.3 控制帧

- **KP-49** ACK 帧：单播数据帧的链路层确认，SIFS 后发送
- **KP-50** RTS / CTS：大帧发送前的信道预约机制，解决隐藏节点问题
- **KP-51** Block ACK（BA / BAR）：802.11n 引入的块确认机制，提升高吞吐效率
- **KP-52** PS-Poll：节电模式下 STA 向 AP 轮询缓存帧
- **KP-53** CF-End：PCF 模式下结束无竞争期

### 2.4 数据帧

- **KP-54** 四地址格式详解：Infrastructure 模式下 ToDS/FromDS 位决定地址语义
- **KP-55** Null 数据帧：STA 通知 AP 进入/退出节电模式，零字节 payload
- **KP-56** QoS 数据帧：802.11e 引入，TID（流量标识符）字段区分 AC（Access Category）
- **KP-57** MSDU 聚合（A-MSDU）与 MPDU 聚合（A-MPDU）：802.11n 效率优化核心

### 2.5 连接状态机

- **KP-58** State 1（未认证 / 未关联）：仅可收发 Class 1 帧（Probe / Beacon / Auth）
- **KP-59** State 2（已认证 / 未关联）：可收发 Class 1+2 帧（增加 AssocReq/Resp）
- **KP-60** State 3（已认证 / 已关联）：完整通信状态，可收发所有 Class 帧
- **KP-61** Deauth 触发 State 3→State 1 回退：强制客户端重新走完整连接流程
- **KP-62** Class 3 帧被 State 2 设备收到时的强制 Deauth 响应

### 2.6 MAC 子层竞争机制

- **KP-63** CSMA/CA：先侦听后发送，冲突发生后随机退避
- **KP-64** DCF（分布式协调功能）：标准竞争模式，基于退避窗口
- **KP-65** EDCA（增强分布式信道接入）：802.11e QoS，四个 AC（Voice/Video/BE/BK）
- **KP-66** 帧间间距：SIFS（最短）< PIFS < DIFS，控制不同优先级的接入
- **KP-67** 竞争窗口（CW）：退避随机数上限，冲突后翻倍（BEB 二进制指数退避）
- **KP-68** NAV（网络分配矢量）：虚拟信道占用机制，防止竞争浪费

---

## 第三章：认证与加密体系

### 3.1 WEP（历史 · 已废弃）

- **KP-69** WEP 核心：RC4 流加密 + 24-bit IV（初始化向量），密钥 40/104 bit
- **KP-70** WEP 致命弱点：IV 空间仅 16M，高流量下 IV 必然复用，统计破解
- **KP-71** FMS 攻击（2001）：利用弱密钥（弱 IV）统计恢复 WEP 密钥
- **KP-72** PTW 攻击（2007）：改进的 WEP 破解算法，仅需 40,000 个包即可破解
- **KP-73** Caffe Latte 攻击：无需关联 AP，诱导客户端发出 ARP 请求进行离线 WEP 破解
- **KP-74** Chopchop 攻击：逐字节解密 WEP 加密帧，可实现明文恢复

### 3.2 WPA（过渡方案 · 已不推荐）

- **KP-75** WPA = 802.11i 草案 + TKIP 加密（RC4 + 动态密钥 + MIC）
- **KP-76** TKIP（Temporal Key Integrity Protocol）：动态密钥混合 + 序列计数器防重放
- **KP-77** Michael MIC 完整性校验的弱点：可被伪造，触发反措施（MIC Failure）
- **KP-78** TKIP 破解：ChopChop 变种 / Beck-Tews 攻击，部分场景可解密短包

### 3.3 WPA2（802.11i · 当前主流）

- **KP-79** CCMP（CTR with CBC-MAC Protocol）：基于 AES-128，是 WPA2 强度保证
- **KP-80** 成对主密钥（PMK）：来源于 PSK（PSK 模式）或 802.1X（Enterprise 模式）
- **KP-81** 4-Way Handshake（四次握手）：ANonce + SNonce 生成 PTK（成对临时密钥）
- **KP-82** PTK 派生：PTK = PRF-512(PMK, ANonce, SNonce, BSSID, Client MAC)
- **KP-83** PTK 拆分：KCK（确认密钥）/ KEK（加密密钥）/ TK（传输密钥）
- **KP-84** GTK（组临时密钥）：第 3 帧由 AP 加密分发，用于广播/组播解密
- **KP-85** PMKID 字段：PMKID = HMAC-SHA1(PMK, "PMK Name" || BSSID || Client MAC)
- **KP-86** 密钥重装攻击 KRACK（2017）：重放握手第 3 帧，强制 Nonce 复用
- **KP-87** WPA2-PSK 的根本弱点：弱密码可被离线字典暴力攻击，强密码则极难破解
- **KP-88** 混合模式（WPA/WPA2）：降级风险，仍可被 TKIP 攻击

### 3.4 WPA3（现代标准）

- **KP-89** SAE（Simultaneous Authentication of Equals）：替代 PSK，基于 Dragonfly 协议
- **KP-90** SAE 核心优势：具备前向保密性（PFS），离线字典攻击失效
- **KP-91** SAE 的已知弱点：Dragonblood 攻击（2019）—侧信道 + 降级攻击
- **KP-92** OWE（Opportunistic Wireless Encryption）：开放网络透明加密，无需密码
- **KP-93** WPA3-Enterprise 192 位：Suite-B 套件，适用于政府 / 金融级场景
- **KP-94** WPA3 Transition Mode：向下兼容 WPA2 客户端，SAE+PSK 并行
- **KP-95** Easy Connect（DPP）：二维码/NFC 配网协议，替代 WPS

### 3.5 802.1X / EAP 企业级认证

- **KP-96** 三方架构：Supplicant（客户端）/ Authenticator（AP）/ Authentication Server（RADIUS）
- **KP-97** EAP-TLS：双向证书认证，最安全，部署复杂，无密码泄露风险
- **KP-98** EAP-TTLS：TLS 隧道内承载 Legacy 认证（PAP/MS-CHAPv2），无需客户端证书
- **KP-99** PEAP（Protected EAP）：TLS 隧道内承载 EAP-MSCHAPv2，微软生态常见
- **KP-100** EAP-FAST：思科提出，使用 PAC（受保护访问凭据）替代证书
- **KP-101** MS-CHAPv2 致命弱点：挑战-响应可离线破解（asleap / Hashcat NetNTLMv2）
- **KP-102** RADIUS 服务器配置：端口 1812（认证）/ 1813（计费），共享密钥管理

### 3.6 802.11w 受保护管理帧（PMF）

- **KP-103** PMF 核心：对管理帧（Deauth / Disassoc / Action）提供完整性保护
- **KP-104** BIP（广播完整性协议）：IGTK 密钥 + AES-CMAC 签名组播管理帧
- **KP-105** IGTK（完整性组临时密钥）：在 4-Way Handshake 第 3 帧中分发
- **KP-106** PMF 两种模式：`capable`（可选，允许非 PMF 客户端）/ `required`（强制）
- **KP-107** PMF 对 Deauth 攻击的防护效果：启用后广播 Deauth 帧无效，单播帧需 MIC 校验
- **KP-108** PMF 的局限：SA Query 机制可被中间人绕过的场景分析
- **KP-109** WiFi 6 强制要求 PMF：`required` 模式是 WPA3 的前提条件

---

## 第四章：WiFi 攻击技术全谱

### 4.1 被动侦察

- **KP-110** 信道跳变扫描（Channel Hopping）：按信道轮询捕获全频段 Beacon/Probe
- **KP-111** AP 信息提取：SSID / BSSID / 加密类型 / 信道 / 支持速率 / 厂商（OUI）
- **KP-112** Probe Request 监听：捕获客户端历史已知网络列表（PNL）用于 Karma 攻击
- **KP-113** 隐藏 SSID 发现：监听客户端 Probe Request 或 Association Request 中暴露的真实 SSID
- **KP-114** 被动 OS 指纹识别：通过 Probe Request 中的 IE 列表和速率集推断设备操作系统
- **KP-115** Beacon 帧时序分析：识别 AP 重启 / 配置变更 / 信道切换事件

### 4.2 Deauthentication 攻击

- **KP-116** Deauth 帧注入原理：伪造来自 AP 的 Deauth 帧，强制客户端断线
- **KP-117** 广播 Deauth vs 单播 Deauth：广播目标 FF:FF:FF:FF:FF:FF 效率高但部分设备忽略
- **KP-118** Deauth Reason Code 选择：Code 3（STA Leaving ESS）最常见，Code 7 触发状态机回退
- **KP-119** 利用 Deauth 触发 Handshake 捕获：强制客户端重连，捕获 4-Way Handshake
- **KP-120** Channel Switch Announcement（CSA）攻击：伪造 Action 帧通知客户端切换信道
- **KP-121** 802.11w PMF 对 Deauth 的防护机制（见 KP-107）

### 4.3 WPA Handshake 捕获与破解

- **KP-122** EAPOL 四次握手抓包：airodump-ng + aireplay-ng Deauth 强制触发
- **KP-123** PMKID 无客户端捕获：hcxdumptool 直接从 AP 的 EAPOL-1 帧中提取 PMKID
- **KP-124** Hashcat 格式转换：hcxtools → .hc22000 格式
- **KP-125** 字典攻击（Dictionary Attack）：wordlist 逐一计算 PMK，与 MIC 比对
- **KP-126** 暴力攻击（Brute Force）：有限字符集穷举，适用于短密码
- **KP-127** 规则变换（Rule-Based Attack）：Hashcat Rules 对字典词汇进行大小写/追加/替换变换
- **KP-128** GPU 加速破解：Hashcat 利用 GPU 并行算力，RTX 4090 可达 300+ kH/s（WPA2）
- **KP-129** Rainbow Table 攻击：预计算 SSID+密码组合，以空间换时间
- **KP-130** 常用 Wordlist：rockyou.txt（14M 条）/ SecLists / CrackStation
- **KP-131** 密码强度保证：随机 12+ 位混合密码在当前算力下破解成本超 1000 年

### 4.4 Evil Twin / Rogue AP 攻击

- **KP-132** Evil Twin 核心原理：克隆合法 AP 的 SSID（可选相同 BSSID），广播更强信号
- **KP-133** Hostapd 配置：创建任意参数的软 AP（SSID / BSSID / 信道 / 加密类型）
- **KP-134** 配套 DHCP 服务（dnsmasq）：为连接的受害设备分配 IP，建立网络栈
- **KP-135** DNS 劫持重定向：dnsmasq address 配置将任意域名解析到攻击者 IP
- **KP-136** Captive Portal（强制认证门户）：伪造 WiFi 密码页面，收集明文凭据
- **KP-137** 客户端强制接入策略：配合 Deauth 攻击驱离合法 AP 上的客户端
- **KP-138** Open Evil Twin vs WPA Evil Twin：开放网络自动连接率高，WPA 需要客户端无脑降级
- **KP-139** Airgeddon 框架：一键化 Evil Twin + Captive Portal 工作流自动化

### 4.5 Karma 攻击

- **KP-140** Karma 攻击本质：响应设备发出的所有 Probe Request（不论 SSID），伪装成任意 AP
- **KP-141** Known Beacons 攻击：主动广播常见 SSID 列表，诱导未发出 Probe 的设备连接
- **KP-142** Loud Karma：不区分定向 Probe 和广播 Probe，响应所有探测
- **KP-143** 防御方法：设备端随机化 MAC 地址 + 随机化 Probe Request SSID（iOS 14+ 默认开启）
- **KP-144** GhostESP Karma 实现：自动学习 PNL + 可指定自定义 SSID 列表

### 4.6 中间人（MITM）攻击

- **KP-145** ARP 欺骗（ARP Spoofing）：伪造 ARP Reply，将网关 MAC 替换为攻击者 MAC
- **KP-146** DNS 欺骗（DNS Spoofing）：拦截 DNS 查询，返回恶意 IP
- **KP-147** SSL Strip：将 HTTPS 链接降级为 HTTP，拦截明文凭据（依赖 HSTS 未部署的目标）
- **KP-148** Bettercap 框架：集成 ARP 欺骗 / DNS 欺骗 / HTTP 代理 / SSL Strip 的 MITM 工具链
- **KP-149** sslstrip2 / bettercap-sslstrip：绕过部分 HSTS 预加载的实现
- **KP-150** 流量转发配置：iptables MASQUERADE + ip_forward，使受害者流量正常通过攻击者

### 4.7 WPS 攻击

- **KP-151** WPS（WiFi Protected Setup）：8 位 PIN 实际有效空间仅 10^4 × 10^3 = 11000 次（校验位）
- **KP-152** WPS 在线暴力破解：Reaver / Bully 工具，速度受 AP 限速影响（通常数小时）
- **KP-153** Pixie Dust 攻击（离线）：利用部分 AP 实现中 E-S1/E-S2 随机数预测缺陷，秒级破解
- **KP-154** WPS Lock 机制：连续失败后 AP 锁定 WPS，需重启解除
- **KP-155** WPS 防御：在路由器设置中永久禁用 WPS 功能

### 4.8 DoS / 泛洪攻击

- **KP-156** Deauth / Disassoc 泛洪：持续广播 Deauth 帧，无法连接（Denial of Service）
- **KP-157** Beacon 洪泛（SSID 轰炸）：MDK4 模式 b，广播数千个随机 SSID，混淆 WiFi 列表
- **KP-158** Authentication 洪泛：MDK4 模式 a，耗尽 AP 的连接状态表
- **KP-159** Probe Response 洪泛：虚假响应占满信道
- **KP-160** EAPOL-Logoff 攻击：伪造 802.1X 注销帧，强制 Enterprise 用户断开
- **KP-161** SAE 洪泛：针对 WPA3 AP，消耗 Dragonfly 握手算力资源
- **KP-162** RF 干扰（物理层 DoS）：宽带噪声发生器，合法性存在严重问题

### 4.9 GTK 组密钥滥用测试

- **KP-163** 客户端隔离（Client Isolation）原理：AP 阻止同一 BSS 内客户端直接通信
- **KP-164** GTK Abuse：利用组播密钥（GTK）构造二层广播包，绕过隔离发送到其他客户端
- **KP-165** GhostESP GTK Abuse Test：验证目标 AP 的客户端隔离是否可被绕过
- **KP-166** 防御：AP 侧正确实现客户端隔离 + 阻止组播转单播转发

### 4.10 WPA2-Enterprise 专项攻击

- **KP-167** Hostapd-WPE（Wireless Pwnage Edition）：伪造 RADIUS 服务器，捕获 PEAP/EAP-TTLS 握手
- **KP-168** MS-CHAPv2 哈希捕获：从 EAP 握手中提取 NetNTLMv2，离线破解
- **KP-169** GTC Downgrade 攻击：将 EAP 类型从 MS-CHAPv2 降级为 GTC，获取明文密码
- **KP-170** 证书验证绕过：客户端未配置服务器证书 CN 验证，接受任意证书
- **KP-171** EAPHammer 工具：集成上述攻击的自动化框架

---

## 第五章：WiFi 防御技术体系

### 5.1 PMF 部署策略

- **KP-172** 全网强制 PMF Required 模式，拒绝不支持 PMF 的旧设备连接
- **KP-173** AP 端验证 PMF 协商状态，记录 SA Query 失败告警

### 5.2 WPA3 迁移路径

- **KP-174** WPA3 Transition Mode：WPA2+WPA3 并行，兼容旧设备同时提升安全性
- **KP-175** OWE 部署：替换公共开放网络（Guest WiFi），实现零配置加密
- **KP-176** 禁用 WPA / TKIP / WEP：从控制台明确配置只允许 WPA2/WPA3

### 5.3 无线入侵检测（WIDS）

- **KP-177** 基于签名的管理帧异常检测：Deauth 泛洪 / Beacon 泛洪 / Auth 泛洪
- **KP-178** Rogue AP 检测：相同 SSID + 不同 BSSID / 非法信道的 AP 告警
- **KP-179** Evil Twin 检测：监测 SSID 与已知 BSSID 的绑定异常
- **KP-180** Kismet 作为轻量级 WIDS：规则配置、告警输出、日志分析
- **KP-181** 专用 WIPS 硬件方案（思科 Cisco WIPS / Aruba WIPS）

### 5.4 网络隔离与分段

- **KP-182** 客户端隔离（Client Isolation）：防止同 BSS 内横向移动
- **KP-183** Guest VLAN 分段：访客网络与内网完全隔离，802.1Q 标记
- **KP-184** 零信任无线：每客户端独立 VLAN + 最小权限 ACL
- **KP-185** 禁止 Guest 网络出口 IP 与内网出口相同（防 IP 追踪）

### 5.5 认证与接入控制强化

- **KP-186** WPA2/WPA3-Enterprise 替换 PSK：消除共享密钥泄露风险
- **KP-187** EAP-TLS 强制证书双向认证：客户端必须持有合法证书
- **KP-188** RADIUS 服务器硬化：限制 IP 白名单 / 加密通信 / 日志审计
- **KP-189** MAC 地址过滤（辅助手段）：可被欺骗，仅作为基础过滤层
- **KP-190** 无线访问时间策略：非工作时间自动关闭 SSID 广播
- **KP-191** PSK 密码策略：最少 20 位随机字符，定期轮换，禁止使用默认值

---

## 第六章：流量捕获与深度分析

### 6.1 监听模式（Monitor Mode）

- **KP-192** Monitor Mode vs Promiscuous Mode：Monitor 工作于链路层，捕获所有帧含管理/控制帧
- **KP-193** Linux 启用 Monitor Mode：`sudo airmon-ng start wlan0` 生成 `wlan0mon`
- **KP-194** iw 现代命令：`iw dev wlan0 set monitor control`，避免 NetworkManager 冲突
- **KP-195** 兼容 Monitor Mode 的芯片组推荐：Mediatek MT7612U / Realtek RTL8812AU

### 6.2 Wireshark 802.11 高级分析

- **KP-196** wlan.fc.type_subtype 过滤字段：0x08=Beacon / 0x0b=Auth / 0x00=Assoc Req / 0x0c=Deauth
- **KP-197** EAPOL 4-Way Handshake 过滤：`eapol` 显示所有握手帧
- **KP-198** Radiotap 头部解析：信号强度（dBm）/ 信道频率 / 数据速率 / 天线 / MCS
- **KP-199** WPA2 实时解密：Wireshark 中配置 PSK，可对已捕获 EAPOL 的流量实时解密
- **KP-200** WLAN Statistics 功能：按 SSID/BSSID 聚合统计，快速定位活跃 AP
- **KP-201** Coloring Rules 自定义：对 Deauth / Auth Fail / Beacon 分色高亮

### 6.3 工具链

- **KP-202** airodump-ng：按指定 BSSID + 信道过滤捕获，输出 .cap / .csv / .kismet 文件
- **KP-203** hcxdumptool：高效 PMKID + Handshake 捕获，支持过滤和信道调度
- **KP-204** tshark：Wireshark 的命令行版本，适合脚本化批量分析
- **KP-205** tcpdump：系统级轻量抓包，`-i wlan0mon` 配合 Monitor Mode 使用
- **KP-206** Scapy（Python）：802.11 帧构造与解析库，可编程实现定制化协议分析
- **KP-207** PyShark：Wireshark/tshark 的 Python 封装，适合自动化 PCAP 分析

### 6.4 数据包注入

- **KP-208** aireplay-ng 注入模式：--deauth / --fakeauth / --arpreplay / --chopchop / --fragment
- **KP-209** 注入测试：`aireplay-ng -9 wlan0mon`，验证网卡是否支持注入
- **KP-210** Scapy 802.11 注入：`sendp(RadioTap()/Dot11()/frame, iface='wlan0mon')`

---

## 第七章：嵌入式平台实现（ESP32 / ESP8266）

### 7.1 ESP32 WiFi 驱动架构

- **KP-211** 乐鑫 WiFi 栈三种工作模式：STA（客户端）/ AP（热点）/ STA+AP（并发）
- **KP-212** 混杂模式（Promiscuous Mode）：`esp_wifi_set_promiscuous(true)`，接收所有帧
- **KP-213** 802.11 帧过滤器配置：`wifi_promiscuous_filter_t`，按帧类型过滤
- **KP-214** 混杂模式回调：`esp_wifi_set_promiscuous_rx_cb(rx_callback)`，帧头含 Radiotap 等价元数据
- **KP-215** 原始帧发送：`esp_wifi_80211_tx(WIFI_IF_STA, frame, len, en_sys_seq)` 注入任意 802.11 帧
- **KP-216** 信道设置：`esp_wifi_set_channel(channel, bandwidth)` 切换监听信道

### 7.2 关键实现细节

- **KP-217** `wifi_pkt_rx_ctrl_t` 结构体：对应 Radiotap，提供 RSSI / 信道 / 速率 / 聚合标志
- **KP-218** 信道跳变（Channel Hopping）：FreeRTOS TimerTask 定时调用 esp_wifi_set_channel
- **KP-219** MAC 地址欺骗：`esp_wifi_set_mac(WIFI_IF_AP, custom_mac)` 克隆目标 AP 的 BSSID
- **KP-220** AP 克隆防 Deauth 方案：克隆 AP 自动对 Class 3 帧回应 Deauth，无需注入

### 7.3 ESP32 系列芯片对比

- **KP-221** ESP8266：单核 Tensilica L106 @ 80MHz，无 BLE，SDK 限制较多，价格极低
- **KP-222** ESP32（原版）：双核 Xtensa LX6 @ 240MHz，支持 WiFi + BT Classic + BLE
- **KP-223** ESP32-S2：单核 + USB OTG，无蓝牙，适合 USB HID 攻击载荷
- **KP-224** ESP32-S3：双核 + AI 加速 + USB OTG + 支持 BLE 5.0，GhostESP 主力硬件
- **KP-225** ESP32-C3：单核 RISC-V，性价比最高，支持 WiFi 4 + BLE 5.0
- **KP-226** ESP32-C6：支持 WiFi 6（802.11ax）+ BLE 5.3 + Thread / Zigbee

### 7.4 ESP-IDF 开发关键组件

- **KP-227** FreeRTOS 任务管理：xTaskCreatePinnedToCore 指定 CPU 核心，隔离 WiFi 与业务任务
- **KP-228** NVS（Non-Volatile Storage）：Flash 键值存储，持久化配置与 MAC 日志
- **KP-229** SPIFFS / LittleFS：文件系统层，存储 Evil Portal HTML / 捕获的 PCAP
- **KP-230** 事件循环（Event Loop）：统一的异步事件分发机制（WIFI_EVENT / IP_EVENT）
- **KP-231** ESP-IDF WiFi API 文档：`docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/network/esp_wifi.html`

---

## 第八章：战争驾驶与无线 OSINT

### 8.1 战争驾驶基础

- **KP-232** 战争驾驶（Wardriving）：移动中采集 WiFi AP 的 SSID/BSSID/加密/信道/GPS 信息
- **KP-233** 战争步行（Warwalking）/ 战争飞行（Warflying）：载体形式变种
- **KP-234** WiGLE CSV 格式规范：MAC / SSID / AuthMode / FirstSeen / Channel / RSSI / Lat / Lon / Alt

### 8.2 工具

- **KP-235** Kismet：跨平台，支持多数据源，内置 PCAP 日志 + GPS + Web UI + WIDS 规则
- **KP-236** GhostESP Wardriving 模式：结合 GPS 模块实时记录 + WiGLE CSV 输出
- **KP-237** NetStumbler（历史）：早期 Windows 工具，仅支持 2.4 GHz，已停止维护

### 8.3 WiGLE 平台

- **KP-238** WiGLE（Wireless Geographic Logging Engine）：全球最大无线网络众包数据库
- **KP-239** WiGLE API 上传：Base64(name:token) 格式认证，自动去重防重复贡献
- **KP-240** WiGLE 数据应用：地理可视化 / AP 密度热力图 / 特定地点历史 AP 追踪

### 8.4 无线 OSINT

- **KP-241** SSID 命名规律分析：从 SSID 推断设备类型（ISP 默认 SSID → 路由器型号 → 漏洞库比对）
- **KP-242** BSSID OUI 查询：`macvendorlookup.com`，从前 3 字节识别设备厂商
- **KP-243** 企业 WiFi 变更监控：WiGLE 历史数据 diff，发现 AP 替换 / 新 SSID 上线事件

---

## 第九章：蓝牙安全

### 9.1 蓝牙协议基础

- **KP-244** BR/EDR（经典蓝牙）vs BLE（低功耗蓝牙）：前者高带宽，后者低功耗，协议栈不同
- **KP-245** BLE 广播机制（Advertising）：ADV 信道 37/38/39，1 秒内周期广播
- **KP-246** GATT（Generic Attribute Profile）：BLE 的服务 / 特征 / 描述符层次结构
- **KP-247** GAP（Generic Access Profile）：控制广播模式和连接建立

### 9.2 BLE 广播帧类型

- **KP-248** ADV_IND：可连接非定向广播，最常见
- **KP-249** ADV_NONCONN_IND：不可连接广播，用于 Beacon（iBeacon / Eddystone）
- **KP-250** SCAN_REQ / SCAN_RSP：主动扫描后的附加数据交换
- **KP-251** iBeacon 格式解析：UUID / Major / Minor / TX Power，用于室内定位
- **KP-252** Eddystone 格式：URL / UID / TLM 三种帧类型

### 9.3 BLE 安全机制

- **KP-253** Legacy Pairing（BLE 4.x）：Just Works（无安全）/ Passkey Entry / OOB，均存在 MITM 风险
- **KP-254** LE Secure Connections（BLE 4.2+）：ECDH 密钥交换，抵抗被动监听
- **KP-255** BLE MAC 地址随机化：可解析私有地址（Resolvable Private Address）每 15 分钟轮换
- **KP-256** BLE 跟踪绕过：通过广播帧内容（Service UUID / 厂商特定数据）的指纹关联跨轮换追踪

### 9.4 蓝牙攻击技术

- **KP-257** BlueSnarfing：未经授权读取蓝牙设备中的联系人/日历数据（蓝牙 1.x 时代漏洞）
- **KP-258** Bluebugging：远程控制蓝牙设备的 AT 命令接口
- **KP-259** BLE MITM：利用 Just Works 配对的无认证弱点
- **KP-260** KNOB 攻击（2019）：降低 BR/EDR 加密熵至 1 字节，暴力破解会话密钥
- **KP-261** Apple AirTag 滥用追踪：Find My 网络的 BLE 广播 + 密钥轮换机制分析

### 9.5 蓝牙安全工具

- **KP-262** Ubertooth One：开源 2.4 GHz 射频硬件，可监听 BR/EDR 通信
- **KP-263** nRF Sniffer（Nordic）：BLE 专用嗅探工具，配合 Wireshark 使用
- **KP-264** Bettercap BLE 模块：`ble.recon on` 扫描，`ble.enum target` 枚举 GATT
- **KP-265** GhostESP BLE 扫描：广播帧解析 + GATT 发现 + AirTag 检测

---

## 第十章：Sub-GHz 与其他无线协议

### 10.1 Sub-GHz 基础

- **KP-266** 常用频段：315 MHz（北美车钥匙）/ 433 MHz（欧洲家用）/ 868 MHz（欧洲 LoRa）/ 915 MHz（北美 LoRa）
- **KP-267** 调制方式：OOK（开关键控）/ ASK（幅移键控）/ FSK（频移键控）/ GFSK
- **KP-268** 固定码 vs 滚动码：固定码（简单遥控器）重放即有效，滚动码每次变换

### 10.2 典型协议与应用

- **KP-269** 汽车遥控钥匙：OOK/FSK 调制，滚动码（KeeLoq / AUT64）防重放
- **KP-270** 车库门遥控：早期使用固定码，现代使用滚动码
- **KP-271** LoRa / LoRaWAN：长距离（10+ km）低功耗，用于 IoT 传感器网络
- **KP-272** Z-Wave：868/915 MHz，智能家居协议，Mesh 拓扑
- **KP-273** Zigbee：2.4 GHz，IEEE 802.15.4，智能家居 / 工业 IoT

### 10.3 Sub-GHz 攻击技术

- **KP-274** 信号重放攻击：捕获固定码遥控信号，重放开门/解锁
- **KP-275** RollJam 攻击（Samy Kamkar）：干扰接收 → 存储用户按下的两帧 → 重放第二帧
- **KP-276** 降频中继攻击（RELAY Attack）：放大汽车钥匙信号，使车辆误判钥匙在附近

### 10.4 工具

- **KP-277** RTL-SDR：低成本 USB 软件无线电接收器，20 MHz 实时带宽
- **KP-278** HackRF One：发射 + 接收，1 MHz–6 GHz，适合 Sub-GHz 研究
- **KP-279** Flipper Zero：集成多协议无线工具，Sub-GHz / RFID / NFC / IR / BLE
- **KP-280** GhostESP Sub-GHz 功能（配合 CC1101 模块）：扫描 / 捕获 / 重放
- **KP-281** GNU Radio：开源信号处理框架，可实现任意协议的软件解调

---

## 第十一章：无人机与空中安全

### 11.1 OpenDroneID 协议

- **KP-282** FAA RemoteID 规范（美国）：2023 年 9 月起强制要求，无人机须广播身份信息
- **KP-283** OpenDroneID WiFi 广播模式：Beacon 帧中携带 Vendor Specific IE（0xFA0BBC）
- **KP-284** OpenDroneID BLE 广播模式：基于 BLE ADV 帧，Manufacturer Specific Data
- **KP-285** RemoteID 数据字段：无人机 ID / 操控者 GPS / 无人机 GPS / 高度 / 速度 / 航向 / 时间戳
- **KP-286** DJI 私有协议：DJI Drone ID，与 OpenDroneID 并行，可被 GhostESP 解析

### 11.2 无人机探测

- **KP-287** GhostESP Aerial Detection：WiFi 扫描阶段发现 OpenDroneID Beacon / DJI WiFi AP
- **KP-288** BLE 阶段检测：解析 OpenDroneID BLE 广播 + DJI BLE 广播
- **KP-289** 多传感器融合：WiFi + BLE 交叉验证，降低误报率
- **KP-290** ADS-B 监听（固定翼）：1090 MHz，RTL-SDR + dump1090 实现航班追踪

### 11.3 反无人机技术概述

- **KP-291** RF 定向干扰：需要频率许可证，执法/军事场景专用，民用严格限制
- **KP-292** GPS 欺骗（Spoofing）：向无人机发送虚假 GPS 信号，迫使返航到错误位置（高度危险）
- **KP-293** 网络拓扑分析：通过 RemoteID 数据实时追踪无人机操控者的 GPS 位置

---

## 第十二章：工具链与研究环境

### 12.1 Linux 无线系统工具

- **KP-294** `iw`：现代无线接口管理（设置 Monitor Mode / 信道 / 功率）
- **KP-295** `iwconfig`：传统工具，功能受限，部分场景仍需使用
- **KP-296** `rfkill`：软硬件无线电开关状态管理
- **KP-297** `airmon-ng check kill`：杀死 NetworkManager / wpa_supplicant 等干扰进程
- **KP-298** `ip link / ip addr`：现代替代 ifconfig 的网络接口管理命令

### 12.2 Kali Linux 无线安全工具集

| 工具 | 核心功能 |
|------|---------|
| Aircrack-ng 套件 | Monitor Mode / 抓包 / Deauth 注入 / WEP-WPA 破解 |
| Kismet | 被动扫描 / WIDS / GPS / BLE / 多协议支持 |
| Bettercap | ARP 欺骗 / DNS 欺骗 / SSL Strip / BLE / Evil Twin |
| Hostapd / Hostapd-WPE | 创建软 AP / 伪造 RADIUS |
| Wifiphisher | 自动化 Evil Twin + Captive Portal |
| Airgeddon | 全流程无线攻击框架（菜单驱动） |
| MDK3 / MDK4 | Beacon 洪泛 / Deauth 洪泛 / Auth 洪泛 |
| hcxdumptool | PMKID / Handshake 高效捕获 |
| hcxtools | 格式转换（.hc22000 for Hashcat） |
| Reaver / Bully | WPS PIN 暴力破解 + Pixie Dust |
| Fluxion | Captive Portal 自动化框架 |
| EAPHammer | WPA2-Enterprise 专项攻击 |

### 12.3 密码破解工具

- **KP-299** Hashcat：GPU 加速，支持 WPA2 (-m 22000) / NetNTLMv2 / MD5 等 300+ 哈希类型
- **KP-300** John the Ripper：CPU 破解，支持规则变换，跨平台
- **KP-301** Aircrack-ng：CPU WPA 字典破解，适合快速验证短密码
- **KP-302** Cowpatty：专用 WPA 破解工具，支持 PBKDF2 预计算
- **KP-303** Pyrit：GPU 加速 WPA 破解（已较少维护）

### 12.4 Python 无线库

- **KP-304** Scapy：802.11 帧构造 / 发送 / 解析，`Dot11 / Dot11Beacon / Dot11Deauth` 类族
- **KP-305** PyShark：tshark 的 Python 封装，用于自动化 PCAP 批量分析

### 12.5 推荐无线网卡（支持 Monitor Mode + 注入）

- **KP-306** Alfa AWUS036ACH（MT7610U）：双频，Linux 原生驱动
- **KP-307** Alfa AWUS036ACHM（MT7612U）：高增益，Kali 首选，Driver 稳定
- **KP-308** Panda PAU09（RT5572）：双频，长距离，Linux 开箱即用
- **KP-309** TP-Link TL-WN722N v1（AR9271）：仅限 v1，v2/v3 芯片已更换不支持注入

---

## 附录一：知识点索引速查

| 编号段 | 章节 | 知识点数量 |
|--------|------|-----------|
| KP-01 ~ KP-33 | 射频与物理层基础 | 33 |
| KP-34 ~ KP-68 | 802.11 协议栈 | 35 |
| KP-69 ~ KP-109 | 认证与加密体系 | 41 |
| KP-110 ~ KP-171 | WiFi 攻击技术全谱 | 62 |
| KP-172 ~ KP-191 | WiFi 防御技术 | 20 |
| KP-192 ~ KP-210 | 流量捕获与分析 | 19 |
| KP-211 ~ KP-231 | 嵌入式平台实现 | 21 |
| KP-232 ~ KP-243 | 战争驾驶与 OSINT | 12 |
| KP-244 ~ KP-265 | 蓝牙安全 | 22 |
| KP-266 ~ KP-281 | Sub-GHz 与其他无线协议 | 16 |
| KP-282 ~ KP-293 | 无人机与空中安全 | 12 |
| KP-294 ~ KP-309 | 工具链与研究环境 | 16 |
| **合计** | **12 个主章节** | **309 个知识点** |

---

## 附录二：学习路径推荐

```
阶段一（基础认知，2–4 周）
├── 第一章：射频物理层基础（KP-01~33）
└── 第二章：802.11 协议栈（KP-34~68）
    └── 推荐资源：NIST SP 800-97 · IEEE 802.11 Wikipedia · Wireshark SampleCaptures

阶段二（密码学与认证，2–3 周）
└── 第三章：认证与加密体系（KP-69~109）
    └── 推荐资源：802.11i 原文 · WPA3 Dragonfly RFC · KRACK 论文

阶段三（攻防技术，4–6 周）
├── 第四章：WiFi 攻击技术（KP-110~171）
└── 第五章：防御技术（KP-172~191）
    └── 推荐实践：TryHackMe Wifi Hacking 101 · HackTheBox WiFi 课程

阶段四（工具与实现，4–6 周）
├── 第六章：流量捕获分析（KP-192~210）
├── 第七章：ESP32 嵌入式实现（KP-211~231）
└── 推荐实践：GhostESP 源码阅读 · ESP-IDF WiFi API 文档

阶段五（拓展领域）
├── 第八章：战争驾驶（KP-232~243）
├── 第九章：蓝牙安全（KP-244~265）
├── 第十章：Sub-GHz（KP-266~281）
└── 第十一章：无人机安全（KP-282~293）
```

---

## 附录三：核心参考资源

| 资源类型 | 名称 | 地址 |
|---------|------|------|
| 官方标准 | IEEE 802.11 工作组 | ieee802.org/11 |
| 官方标准 | NIST SP 800-97 无线安全指南 | nvlpubs.nist.gov |
| 工具文档 | Aircrack-ng 官方 Wiki | aircrack-ng.org/doku.php |
| 工具文档 | Wireshark 802.11 Wiki | wiki.wireshark.org/Wi-Fi |
| 工具文档 | Kismet 官方文档 | kismetwireless.net/docs |
| 工具文档 | Pi-hole 文档 | docs.pi-hole.net |
| 嵌入式开发 | ESP-IDF WiFi API | docs.espressif.com |
| 数据平台 | WiGLE 全球 WiFi 数据库 | wigle.net |
| 实战平台 | TryHackMe WiFi 课程 | tryhackme.com/room/wifihacking101 |
| 实战平台 | HackTheBox WiFi 课程 | academy.hackthebox.com |
| GitHub | ESP32 WiFi Penetration Tool | risinek.github.io/esp32-wifi-penetration-tool |
| GitHub | GhostESP 项目文档 | docs.ghostesp.net |

---

> ⚠️ **合规声明**：本知识树所有内容仅用于授权安全测试、防御性研究和教育目的。在未经授权的网络上使用攻击性工具在大多数国家和地区属于违法行为。实践任何技术前请确保持有明确书面授权。
