WEB+MQ
=========================


[TOC]

#应用场景
redis cluster 原生集群的使用

##变更日志
###2016-12-17
*   启动集群 sh start.sh
*   停止集群 sh stop.sh 
*   fig up 启动nginx 代理


##主要功能
-   log_by_lua_file 调用 rabbitmq-插入日志数据；
-   
###conf/lua.app/lua.d
-   首先建立路由与消息队列，定义绑定的路由（从需要截取的url 中提取关键字）
-   reqcontent.lua 将url 转换为mq 的 route key, 访问的请求数据推送到mq；
###conf/lua.app/third.lua.lib
-   提供需要的第三方lua 库
-   提供resty.http 库，进行非阻塞提交；
-   提供json 库，进行lua to json 数据的转换；
###conf/nginx/http.d
-   nginx 对lua第三方库的环境配置；
###conf/nginx/server.d
-   定义测试路径proxymq，在测试路径中对提交的消息进行捕获；
###conf/test-sync-log.sh
-   在rabbitmq 平台建立测试路由与队列，并且发送测试消息；



##测试
###测试
进入到fig.xml 目录执行以下命令进行测试；

    fig up -d && fig ps
    
    http://127.0.0.1/proxymq/index.html
    
    http://127.0.0.1:15672/#/queues
    http://127.0.0.1:15672/api/
    
    fig stop && fig rm -v --force
    