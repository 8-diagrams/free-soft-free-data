解决在centos7上安装Rabbit MQ

系统：centos 7

安装：

rpm -ivh https://cdn.jsdelivr.net/gh/8-diagrams/free-soft-free-data/data/er-download.rpm --nodeps --force
 
rpm -ivh https://cdn.jsdelivr.net/gh/8-diagrams/free-soft-free-data/data/socat-1.7.3.2-2.el7.x86_64.rpm --nodeps --force
 
rpm -ivh https://cdn.jsdelivr.net/gh/8-diagrams/free-soft-free-data/data/rabbitmq-server-3.8.6-1.el7.noarch.rpm --nodeps --force

启动 rabbitmq

service rabbitmq-server start

修改配置文件

cat - > /etc/rabbitmq/rabbitmq.conf

```


listeners.tcp.default = 5672

collect_statistics_interval = 10000

## Note: this uses the core `load_definitions` key over
## now deprecated `management.load_definitions`
# load_definitions = /path/to/exported/definitions.json

management.tcp.port = 15672
management.tcp.ip   = 0.0.0.0


management.http_log_dir = /var/log/rabbitmq/http

management.rates_mode = basic

# Configure how long aggregated data (such as message rates and queue
# lengths) is retained.
# Your can use 'minute', 'hour' and 'day' keys or integer key (in seconds)
management.sample_retention_policies.global.minute    = 5
management.sample_retention_policies.global.hour  = 60
management.sample_retention_policies.global.day = 1200

management.sample_retention_policies.basic.minute   = 5
management.sample_retention_policies.basic.hour = 60

management.sample_retention_policies.detailed.10 = 5

```

重启

service rabbitmq-server restart

启动web
rabbitmq-plugins enable rabbitmq_management

查一下用户列表

rabbitmqctl list_users

修改唯一用户guest的密码

rabbitmqctl change_password guest xxxxxxxxx

新增一个用户
```
rabbitmqctl add_user admin xxxxxxx
rabbitmqctl set_user_tags admin administrator
rabbitmqctl set_permissions -p "/" admin ".*" ".*" ".*"
rabbitmqctl list_permissions -p /

rabbitmqctl list_users
```

web 管理界面
http://ip:15672/

