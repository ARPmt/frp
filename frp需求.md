# frp需求
## 2023-07-17
1. 南北向接口的端口号分离，如北向restapi 端口 8000， 南向控制协议端口号 8001
2. 增加控制协议客户端 配置文件，可配置 控制器地址、frp类型（frpc/fprs）
3. 客户端SN自动生成规则（序列号里面可识别操作系统类型，可根据设备的硬件属性生成序列号），获取客户端SN命令
4. 设备结构 用户->设备SN
5. 设备token 认证 实现
6. 编写api接口， 可配置 tcp/udp/http/https/文件共享，客户端配置文件代理配置与连接frps 配置相分离

客户端序列号规则：

设备类型 S/C (s为 frps, c frpc) + 系统类型w/i/a/m/c/u (w 为windows, i  ios, a  android, c  centos, u  ubuntu ..) + 设备ID 16 位
如  frps 客户端   SC9f14b05c10187031
    frpc 客户端   CC9f14b05c10187031

设备类型：
  S： 表示 frps 客户端
  C:  表示 frpc 客户端

系统类型：
  W： 表示windows 客户端
  I:  表示 IOS 客户端
  M： 表示 MACOS 客户端
  A:  表示 android 客户端
  C： 表示 centos 客户端
  U:  表示 Ubuntu 客户端
