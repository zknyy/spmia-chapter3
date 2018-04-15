本做是在原有基础上进行都改进，URL ：https://hub.docker.com/r/zknyy/tmx-licensing-service/

要想docker-compose运行dev环境成功，要保证
1. 修改tmx-parent-pom项目：  dev／docker-compose.yml文件中：
liences服务用dev环境
    environment:
      PROFILE: "dev"
重新build一个docker镜像文件（原因是 license服务中的文件 bootstrap.yml  也必须是dev）
#    image: ch3-thoughtmechanix/licensing-service
    image: zknyy/tmx-licensing-service:chapter3      
镜像文件URL：https://hub.docker.com/r/zknyy/tmx-licensing-service/

重新build的 Dockerfile已经做了更新，使用了自己创建的docker web服务来下载jce库文件，其中 "http://67.209.185.207:32768/jce_policy-8.zip" 就是临时的解决方法（也许直接用ADD跟简单）；
其docker web服务的Dockerfile，和jce库文件都在Enhanced目录下
注意：zknyy/tmx-licensing-service:chapter3 只能对应 dev 环境（如果有时间再做成chapter3-dev  和   chapter3-prod 形式来区分）
      
2. 修改licensing-service项目：原因是 license服务中的文件 bootstrap.yml  也必须是dev， 然后在docker build

3. 对于数据库密码加密：
JCE安装目录 拷贝两个jar文件到（MacOS目录结构） /Library/Java/JavaVirtualMachines/**< jdk_version_of_your_pc >**/Contents/Home/jre/lib/security
改动java.security文件，找到  #crypto.policy=unlimited，并去掉注释即可。

4. 另外补充：本地通过命令行参数覆盖启动App
java -Dspring.cloud.config.uri=http://localhost:8888 -Dspring.profiles.active=dev -jar target/licensing-service-0.0.1-SNAPSHOT.jar
