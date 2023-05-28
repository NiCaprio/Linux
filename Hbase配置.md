# Hbase配置
##Hbase下载
1. 选择Hbase版本
我的虚拟机环境：
Ubuntu 22.10
Hadoop版本：3.2.4
Hbase版本：2.4.17
这里选取自己Hadoop版本对应的Hbase版本进行下载
去官网或者镜像网站
下载bin版本hbase-2.4.17-bin.tar.gz
**注意：一定要选择所支持的版本，否则会不兼容**
![1](vx_images/233520422249690.png)
2. 解压Hbase压缩包并配置环境变量
将hbase-2.4.17-bin.tar.gz压缩包解压至路径 /usr/local
~~~
sudo tar -zxf ~/Downloads/hbase-2.2.2-bin.tar.gz -C /usr/local
~~~
修改解压文件名hbase-2.4.17，改为hbase
~~~
cd /usr/local
sudo mv ./hbase-2.2.2 ./hbase
~~~
把hbase目录权限赋予给hadoop用户：
~~~
cd /usr/local
sudo chown -R hadoop ./hbase
~~~
![2](vx_images/95634322246245.png)
为了方便直接使用Hbase命令，这里配置Hbase环境变量
编辑~/.bashrc文件
**提示：这里用gedit编辑会更方便，没有的话需要安装，~sudo apt install gedit~**
~~~
gedit ~/.bashrc
~~~
在最上方添加下面代码：
~~~
export PATH=$PATH:/usr/local/hbase/bin
~~~
![5](vx_images/38154422241999.png)
执行source命令使上述配置在当前终端立即生效
~~~
source ~/.bashrc
~~~
![6](vx_images/571194422257483.png)
检查Hbase是否安装成功
~~~
cd /usr/local/hbase/bin
hbase version
~~~
![7](vx_images/236414522254985.png)
3. Hbase单机模式配置
进入hbase-env.sh , 配置JAVA环境变量，
先检查自己jdk的安装路径
~~~
echo $JAVA_HOME
~~~
![9](vx_images/347144622258666.png)
将显示出来的路径添加到hbase-env.sh中，并添加配置HBASE_MANAGES_ZK为true，
我的JAVA _HOME =/usr/lib/jvm/java-8-openjdk-amd64
用gedit命令打开并编辑hbase-env.sh，命令如下：
~~~
gedit /usr/local/hbase/conf/hbase-env.sh
~~~
添加如下代码：
~~~
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export HBASE_MANAGES_ZK=true 
~~~
![8](vx_images/573504522236226.png)
记得保存退出
然后配置hbase-site.xml
~~~
gedit /usr/local/hbase/conf/hbase-site.xml
~~~
添加代码：
~~~
<configuration>
        <property>
                <name>hbase.rootdir</name>
                <value>file:///usr/local/hbase/hbase-tmp</value>
        </property>
</configuration>
~~~
接下来测试单机模式是否配置成功
~~~
cd /usr/local/hbase
bin/start-hbase.sh
bin/hbase shell
~~~
![10](vx_images/517534722253802.png)
4. Hbase伪分布式配置
配置hbase-enc.sh
~~~
gedit /usr/local/hbase/conf/hbase-env.sh
~~~
加入如下命令：
~~~
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export HBASE_CLASSPATH=/usr/local/hbase/conf 
export HBASE_MANAGES_ZK=true
~~~
![11](vx_images/217434822247348.png)
相当于在刚才单机模式的配置下加入了HBASE_CLASSPATH
配置hbase-site.xml
~~~
gedit /usr/local/hbase/conf/hbase-site.xml
~~~
加入如下命令：
~~~
<configuration>
        <property>
                <name>hbase.rootdir</name>
                <value>hdfs://localhost:9000/hbase</value>
        </property>
        <property>
                <name>hbase.cluster.distributed</name>
                <value>true</value>
        </property>
        <property>
        <name>hbase.unsafe.stream.capability.enforce</name>
        <value>false</value>
    </property>
</configuration>
~~~
在单机模式中的配置
![12](vx_images/141054922240482.png)
修改后：
![13](vx_images/341835022233516.png)
测试是否配置成功
首先登录ssh，然后启动Hadoop，ssh之前配置了无密码登录，可以参考我的另一篇Hadoop安装文章进行无密码设置ssh登录
~~~
ssh localhost
cd /usr/local/hadoop
./sbin/start-dfs.sh
~~~
输入jps查看Hadoop是否启动成功
![14](vx_images/285105122242463.png)
然后启动Hbase
~~~
cd /usr/local/hbase
bin/start-hbase.sh
~~~
输入jps查看Hbase是否启动成功
![15](vx_images/19115222235348.png)
然后进入shell界面
~~~
hbase shell
~~~
输入list可以查看已存在的表
输入status可以查看服务器状态
![16](vx_images/97035222235957.png)
输入exit退出hbase的shell界面
**注意：要先启动Hadoop然后再启动Hbase
