# Jmeter
## java.net.NoRouteToHostException: Can't assign requested address
原因：服务器分配的客户端连接端口被用尽，无法建立socket连接所致。虽然socket连接正常关闭，但端口不是立即释放，而是处于TIME_WAIT状态。默认60S后释放

解决方法：
>1、查看linux支持的客户端连接端口范围：

	cat /proc/sys/net/ipv4/ip_local_port_range
>2、调低端口释放后的等待时间， 默认为60s， 修改为15~30s

	echo 30 > /proc/sys/net/ipv4/tcp_fin_timeout
>3、修改tcp/ip协议配置， 通过配置/proc/sys/net/ipv4/tcp_tw_resue, 默认为0， 修改为1， 释放TIME_WAIT端口给新连接使用

	echo 1 > /proc/sys/net/ipv4/tcp_tw_reuse
>4、修改tcp/ip协议配置，快速回收socket资源， 默认为0， 修改为1

	echo 1 > /proc/sys/net/ipv4/tcp_tw_recycle
>5、执行：sysctl -p ，使设置立即生效

	sysctl -p
>6、其它命令：
	查看端口占用情况：netstat -anp
	统计占用端口数：netstat -anp | wc -l
	查看允许最大进程数：ulimit -SHn
	
