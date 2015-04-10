# DaoCloud CI

DaoCloud CI 使用基于Docker的容器技术来运行您的测试任务，确保您的任务和其他租户的任务完全隔离并互不干扰。

## 环境&服务

DaoCloud目前支持在 *Ubuntu14.04* 上运行您的测试任务，并为您准备了多种编程语言的测试环境，目前支持的编程语言包括：

* Golang (1.4)
* Python (2.7)
* Ruby (1.9.3)
* Java (openjdk7)

同时，为了方便您准备测试环境，我们还提供了丰富的测试服务：

* MySQL (5.5)
* Redis (2.8)
* MongoDB (2.6)

## 配置您的测试

您可以通过在代码根目录放置一个**daocloud.yml**文件来配置您的测试任务。

### 指定测试镜像

测试镜像代表了您的测试任务运行的环境.

    ＃which build image you want to use, you can select one from daocloud support list
    image: go1.4
	
您可以从下面的支持列表中选择您希望的运行环境（该列表正在不断完善中）

* go: Golang 1.4
* go1.4: Golang 1.4
* python: Python 2.7
* python: Python 2.7
* java: Java openjdk7
* openjdk7: Java openjdk7


	
### 配置测试服务

您可以指定测试任务运行时依赖的服务，指定服务后，测试任务可以用docker link一致的方式去访问它。比如您指定要使用*mysql*, 那么您可以使用*mysql*作为域名来访问服务节点, 或者通过环境变量*MYSQL_PORT_3306_TCP_ADDR*来访问。 

    ＃specify list of services you want to use during your test
    services:
      - mysql
      - mongodb

目前DaoCloud CI支持的服务及访问方式如下表所示（该列表正在不断完善中）

#### MySQL

Version：MySQL 5.5

Docker Link Alias: mysql

Host: mysql

Port: 3306

UserName: root

Password: 不设密码

环境变量：

* MYSQL_PORT = tcp://1.2.3.4:3306
* MYSQL_PORT_3306_TCP_ADDR = 1.2.3.4
* MYSQL_PORT_3306_TCP = tcp://1.2.3.4:3306
* MYSQL_PORT_3306_TCP_PROTO = tcp
* MYSQL_PORT_3306_TCP_PORT = 3306



#### Redis

Version：Redis 2.8

Docker Link Alias: redis

Host: redis

Port: 6379

环境变量：

* REDIS_PORT = tcp://1.2.3.4:6379
* REDIS_PORT_6379_TCP_ADDR = 1.2.3.4
* REDIS_PORT_6379_TCP = tcp://1.2.3.4:6379
* REDIS_PORT_6379_TCP_PROTO = tcp
* REDIS_PORT_6379_TCP_PORT = 6379

#### MongoDB

Version：MongoDB 2.6

Docker Link Alias: mongodb

Host: mongodb

Port: 27017

环境变量：

* MONGODB_PORT = tcp://1.2.3.4:27017
* MONGODB_PORT_27017_TCP_ADDR = 1.2.3.4
* MONGODB_PORT_27017_TCP = tcp://1.2.3.4:27017
* MONGODB_PORT_27017_TCP_PROTO = tcp
* MONGODB_PORT_27017_TCP_PORT = 27017

### 设置测试环境变量

您可以指定运行的环境变量

    ＃specify environment variables 
    env:
      - GOPATH = /go
      - MY_ENV = abc


### 指定测试脚本

您可以配置测试脚本来准备和运行您的测试任务，比如安装一些测试依赖包，准备测试环境，运行测试等。

    ＃which scripts you want to run for test
    script:
      - sudo apt-get update
      - sudo apt-get -y install bzr
      - go get -t ./...
      - go test your-code

### 配置邮件通知

您可以配置邮件通知，掌握最新测试结果信息。

    ＃which your email and what condition you want a notice
    notify:
      email:
        recipients:
          - sample@daocloud.io
        on_success: on
        on_failure: off
