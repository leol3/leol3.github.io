---
title: 判断tcp连接是否可用的方法
date: 2020-11-10 17:13:37
tags:
---
在使用一个长连接的TCP时，如果TCP服务器端接收到TCP的客户端连接过来后，接着服务器端的TCP节点需要对这个客户端进行数据收发，收发时需要判断这个SOCKET是否可用，判断方法有多种。  
方法一：  
```
struct tcp_info info; 
int len=sizeof(info); 
getsockopt(sock, IPPROTO_TCP, TCP_INFO, &info, (socklen_t *)&len); 
if((info.tcpi_state==TCP_ESTABLISHED))  // 则说明未断开
```
方法二：  
使用select系统函数，若远端断开，select返回1，recv返回0则断开。  
```
int lSockFd;
fd_set read_set;
struct timeval t_o;

FD_ZERO(&read_set);
FD_SET(lSockFd, &read_set);
t_o.tv_sec = 1; // 超时秒数
int ret = select(lSockFd + 1, &read_set, NULL, NULL, &t_o);
char buf[100] = {0};
if(ret == 1) {
if(FD_ISSET(lSockFd, &read_set) ){
    count = recv(lSockFd, buf, 100, 0);
        if((count == 0)||(count == -1)){
            // 这两种情况都可认为是链路关闭
        } 
    }
}
```
方法三：  
```
int keepAlive = 1; // 开启keepalive属性
int keepIdle = 60; // 如该连接在60秒内没有任何数据往来,则进行探测 
int keepInterval = 5; // 探测时发包的时间间隔为5 秒
int keepCount = 3; // 探测尝试的次数.如果第1次探测包就收到响应了,则后2次的不再发.

setsockopt(rs, SOL_SOCKET, SO_KEEPALIVE, (void *)&keepAlive, sizeof(keepAlive));
setsockopt(rs, SOL_TCP, TCP_KEEPIDLE, (void*)&keepIdle, sizeof(keepIdle));
setsockopt(rs, SOL_TCP, TCP_KEEPINTVL, (void *)&keepInterval, sizeof(keepInterval));
setsockopt(rs, SOL_TCP, TCP_KEEPCNT, (void *)&keepCount, sizeof(keepCount));
```
设置后，若断开，则在使用该socket读写时立即失败，并返回ETIMEDOUT错误。  
方法四：  
自己实现一个心跳检测，一定时间内未收到自定义的心跳包则标记为已断开。