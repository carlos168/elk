#elk日志监控告警系统安装、配置及使用

一. 工具：
    1. elasticsearch
    2. logstash
    3. kibana
    4. filebeat
    5. elastalert

二. 版本
    version: 6.2.2

三. 告警方式
    微信服务号发送模板消息

四. 安装过程
    1. elasticsearch
        解压安装包：
            tar -zxvf elasticsearch-6.2.2.tar.gz
        剪切安装包到安装目录：
            mv elasticsearch-6.2.2 /usr/local/src/
        进入config目录：
            cd /usr/local/src/elasticsearch/config/
        修改elasticsearch.yml配置文件：
            network栏目中添加：
                network.host: 0.0.0.0
                http.port: 9200
            Various栏目中添加：
                http.cors.enabled: true         #elasticsearch启用cors
                http.cors.allow-origin: "*"     #允许访问IP地址段，*为所有IP都可以访问
        执行启动命令：
            /usr/local/src/elasticsearch/bin/elasticsearch
            提示一下错误：
                ERROR: [2] bootstrap checks failed
                [1]: max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]
                [2]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
            解决办法：
                [1]sudo sysctl -w vm.max_map_count=655360
                    再执行：sysctl -p
                [2]vi /etc/security/limits.conf
                    文件最后添加2行：
                        * hard nofile 65536
                        * soft nofile 65536
        防火墙开放9200端口：
            #sudo iptables -A INPUT -p tcp --dport 9200 -j ACCEPT
            #sudo iptables -A INPUT -p tcp --dport 9100 -j ACCEPT
            iptables -A IN_public_allow -p tcp -m tcp --dport 9200 -m conntrack --ctstate NEW -j ACCEPT
            iptables -A IN_public_allow -p tcp -m tcp --dport 9100 -m conntrack --ctstate NEW -j ACCEPT
            iptables -A IN_public_allow -p tcp -m tcp --dport 5601 -m conntrack --ctstate NEW -j ACCEPT
            cd /etc/sysconfig
            查看是否开放：iptables -L -n 
            保存配置：iptables-save > iptables
        重新启动elasticsearch,浏览器打开：http://*.*.*.*:9200/
        如果正常就ok了。
        安装head插件：
            1) 安装npm：sudo yum install npm
            2) 下载head代码：git clone https://github.com/mobz/elasticsearch-head.git
            3) cd elasticsearch-head
            4) npm install
            5) npm run start
            6) 浏览器打开：http://*.*.*.*:9100/
        安装ik中文分词插件：
            1）进入elasticsearch目录
            2）./bin/elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v6.2.2/elasticsearch-analysis-ik-6.2.2.zip
            3）重启elasticsearch
    2. logstash
        解压安装包、进入config目录，新建logstash.conf文件，写入input、fitler、output
        启动：
        /usr/local/src/logstash/bin/logstash -f /usr/local/src/logstash/config/logstash.conf
    3. kibana
        解压安装包、进入config目录，编辑kibana.yml文件，在文件最后添加：
        elasticsearch.url: "http://*.*.*.*:9200"
        server.host: 0.0.0.0
        启动：
        /usr/local/src/kibana/bin/kibana
        浏览器打开：http://*.*.*.*:5601
        注：需开放5601端口
    4. filebeat
        
    5. elastalert
        安装依赖包：
            yum install -y zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel
       1)升级python到2.7.13
         问题：
            centos7升级python的教程已经有很多了
            比如老版本的python是2.6.6，新安装的python版本是2.7.9
            mv /usr/bin/python /usr/bin/python2.6.6
            mv /usr/local/bin/python /usr/bin/python
            大多都提到了安装新的python后，yum无法运行的解决方法
            #vi /usr/bin/yum
            将文件头部的
            #!/usr/bin/python
            改成
            #!/usr/bin/python2.6.6
            在使用yum 安装软件的时候同样会遇到
            ImportError:No module nameed urlgrabber.grabber
            同上面的方法修改一下文件
            vi /usr/libexec/urlgrabber-ext-down
        2) 配置告警规则
           下载微信告警规则：
            修改./config.yaml配置文件
            修改rules目录下的ymal告警规则文件
            修改elastalert_modules目录下的微信告警python代码 

