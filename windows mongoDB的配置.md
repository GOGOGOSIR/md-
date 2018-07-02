# windows mongoDB的配置

1. 首先先安装mongoDB ，我这里选择的是，自定义安装，我安装的是3.6版的，在D:\mongodb\Server\3.6\

2. 安装完成之后，然后在新建两个文件夹

   a. D:\mongodbData       //  这个文件是用来存储mongoDB的数据的

   b. D:\mongodbLog\mongodb.log         // 这个文件夹是用来存储mongoDB的日志文件的

3. 配置window的环境 ， 这样做的目的是，可以在任何目录下通过mongo链接mongoDB

   1. 打开我的电脑 的属性，找到系统属性，进入高级菜单， 点击环境变量，
   2. 在环境变量的系统变量里找到path变量并单击
   3. 在编辑环境变量这个弹框中单击新建按钮，这里填入你mongoDB的安装路径，然后保存即可

4. 为了便捷，我一般使用配置文件启动mongoDB的服务

   1. 首先在d:\mongodb下新建一个config的文件夹，并新建一个mongodb.conf文件，并编辑啊此文件

      ```
      dbpath=D:\mongodbData           # 数据库文件
      logpath=D:\mongodbLog\mongodb.log    # 日志文件
      logappend=true                        # 日志采用追加模式，配置后mongodb日志会追加到现有的日志文件，不会重新创建一个新文件
      journal=true                        # 启用日志文件，默认启用
      quiet=true                            # 这个选项可以过滤掉一些无用的日志信息，若需要调试使用请设置为 false
      port=27017                            # 端口号 默认为 27017
      ```

   2. 以管理员的身份，运行cmd，进入D:\mongodb\Server\3.6\bin这个目录，然后执行以下命令

      ```
      mongod --config "D:\mongodb\config\mongodb.conf"  --install --serviceName "MongoDB"
      ```

   3. 然后 以管理员的身份运行cmd，并输入以下命令

      ```
      sc create MongoDB binPath= "D:\mongodb\Server\3.6\mongod.exe --service --config=D:\mongodb\config\mongodb.conf" (创建服务)
      ```

   4.  然后在开启mongoDB 服务和链接monogoDB 

      ```
      net start MongoDB  // 开启mongoDB服务 或者 sc start MongoDB
      mongo // 链接mongoDB
      ```

参考文章

https://www.cnblogs.com/weschen/p/8213746.html

开启和关闭服务 windows 命令  net 和 sc的区别 参考文章

https://www.cnblogs.com/qlqwjy/p/8010598.html

