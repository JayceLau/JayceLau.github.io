---

title: Parallels Desktop for Mac网络模式介绍

author: Jayce

date: 2018-08-23 23:20:00

tags: [ReadNote, Translation]

categories: Network

---
Parallels Desktop（以下简称PD）提供三种不同网络模式供用户选择，包括：

- 共享网络（推荐）
- 桥接网络
- Host-Only网络

#### 共享网络

这是虚拟机默认的网络模式，也是推荐的网络模式。它无需任何额外的配置，开箱即用。此时PD用作虚拟机的虚拟路由。

- PD创建了一个隔离的带有DHCP服务器（运行再macOS中）的虚拟子网
- 虚拟机属于具有其自己的IP范围的虚拟子网
- 虚拟机在Mac所在的子网中不可见
- 虚拟机具备完全的Internet访问权限
- 如果Mac连接了VPN，也将被自动地共享给虚拟机

![共享网络拓扑图](https://kb.parallels.com/Attachments/kcs-4833/Shared%20Network.png)

该网络模式满足绝大多数人的需求。

#### 桥接网络

使用此模式时，虚拟机使用虚拟网卡直连Internet。	

- 虚拟机表现Mac所在子网中的另外一台计算机
- DHCP服务器（如路由器）提供给虚拟机一个IP
- 虚拟机可以ping通和看到子网中的其他服务器
- 其他服务器可以ping通和看到虚拟机

![桥接网络拓扑图](https://kb.parallels.com/Attachments/kcs-4833/Bridged%20Network.png)

*注：选择此模式时，PD不再管理网络的连接。*

桥接网络可以在特定的网口上启用，如以太网、Wi-Fi等。

- **Bridged: Ethernet**：对应Mac的以太网适配器
- **Bridged: Wi-Fi**：对应Mac的Wi-Fi适配器
- **Bridged: Default Adapter**：对应Mac的默认的适配器

![PD网络适配器](https://kb.parallels.com/Attachments/kcs-4833/networks.png)

#### Host-Only网络

这个模式与共享网络类似，除了它的子网（如10.37.129.x）与外界隔离之外。工作在host-only模式的虚拟机仅能ping通和看到其他虚拟机以及与网关（10.37.129.1）通信。

![Host-Only网络拓扑图](https://kb.parallels.com/Attachments/kcs-4833/Host-only%20network.png)

#### 参考文档

- 译自[Network modes in Parallels Desktop for Mac](https://kb.parallels.com/en/4948)