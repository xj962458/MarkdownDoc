##  %%z%%1、安装frpc
#### 下载
```bash
wget https://github.xjfyt.workers.dev/https://github.com/fatedier/frp/releases/download/v0.42.0/frp_0.42.0_linux_amd64.tar.gz
echo "" > frpc.ini
```

### 解压
```bash
tar -zxvf ./frp_0.42.0_linux_amd64.tar.gz
```

### 改名
```bash
mv frp_0.42.0_linux_amd64 frpc
```

### 进入
```bash
cd frpc
```

### 删除
```bash
rm -rf ./frps* s* L*
```

### 创建文件
```bash
vim frpc.ini
```

### 编辑
```ini
# [common] is integral section
[common]
# A literal address or host name for IPv6 must be enclosed
# in square brackets, as in "[::1]:80", "[ipv6-host]:http" or "[ipv6-host%zone]:80"
server_addr = 106.54.173.30
server_port = 5443

# for authentication
token = lcww6M9kmgEaE4aQ

# connections will be established in advance, default value is zero
pool_count = 5

# if tcp stream multiplexing is used, default is true, it must be same with frps
tcp_mux = true

# your proxy name will be changed to {user}.{proxy}
user = lab

# decide if exit program when first login failed, otherwise continuous relogin to frps
# default is true
login_fail_exit = false

# console or real logFile path like ./frpc.log
log_file = ./frpc.log
# trace, debug, info, warn, error
log_level = info
log_max_days = 3

# communication protocol used to connect to server
# now it supports tcp and kcp, default is tcp
protocol = tcp

# Resolve your domain names to [server_addr] so you can use http://web01.yourdomain.com to browse web01 and http://web02.yourdomain.com to browse w

[sem]                                                                                                                   
remote_port = 3333                                                                                                    
type = tcp
local_ip = localhost
local_port = 8000
#host_header_rewrite =
# ====================
```