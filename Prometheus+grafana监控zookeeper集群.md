# zookeeper集群配置

在zookeeper的配置文件zoo.cfg中添加如下：

```bash
metricsProvider.className=org.apache.zookeeper.metrics.prometheus.PrometheusMetricsProvider
metricsProvider.httpPort=7000
metricsProvider.exportJvmInfo=true
```

将zookeeper信息暴露在7000端口，此时访问：IP:7000/metrics即可看到实时监控数据，注意此功能只能在zookeeper3.6.0及以上版本开启。

一些有用的信息如下：

```bash
follower_sync_time{quantile="0.5",} NaN
follower_sync_time_count 1.0
follower_sync_time_sum 97.0

znode_count 5.0

watch_count 0.0

connection_drop_count 0.0

dbinittime_count 1.0

election_time_count 1.0
```

# 安装Prometheus

`sudo apt install prometheus`

或者

`yum install prometheus`

配置文件地址为：

`/etc/prometheus/prometheus.yml`

修改如下配置

```yaml
scrape_configs:
  - job_name: 'zookeeper-1'
    static_configs:
      - targets: ['192.168.32.1:7000']
  - job_name: 'zookeeper-2'
    static_configs:
      - targets: ['192.168.32.2:7000']
  - job_name: 'zookeeper-3'
    static_configs:
      - targets: ['192.168.32.3:7000']

```

以上为zookeeper配置信息

启动命令为

`sudo systemctl start prometheus`

启动后可访问IP:9090/targets来访问web页面

![](https://raw.githubusercontent.com/lelelemonade/Images/main/Xnip2020-11-09_03-04-34.jpg)

# 安装grafana

`sudo apt install grafana`

或者

`yum install grafana`

启动命令为

`sudo systemctl start grafana`

启动浏览器访问此时访问：IP:3000端口进入web页面：

添加数据源

![](https://raw.githubusercontent.com/lelelemonade/Images/main/Xnip2020-11-09_02-44-09.jpg)
选择prometheus

![](https://raw.githubusercontent.com/lelelemonade/Images/main/Xnip2020-11-09_02-44-33.jpg)



添加IP地址，如IP:9090

![](https://raw.githubusercontent.com/lelelemonade/Images/main/Xnip2020-11-09_02-44-33.jpg)


添加展示模版，模版id填10465，这是官方模版

![](https://raw.githubusercontent.com/lelelemonade/Images/main/Xnip2020-11-09_02-48-38.jpg)

查看展示效果

![](https://raw.githubusercontent.com/lelelemonade/Images/main/Xnip2020-11-09_02-56-30.jpg)