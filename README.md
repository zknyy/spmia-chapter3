本作是在原有基础上进行都改进，URL ：https://github.com/zknyy/spmia-chapter3

#要想docker-compose运行dev环境成功，要保证
##1. 修改tmx-parent-pom项目：  dev／docker-compose.yml文件中：
liences服务用dev环境
 **   environment:**
 **     PROFILE: "dev"**
重新build一个docker镜像文件（原因是 license服务中的文件 bootstrap.yml  也必须是dev）
**#    image: ch3-thoughtmechanix/licensing-service**
**    image: zknyy/tmx-licensing-service:chapter3   **   
镜像文件URL：https://hub.docker.com/r/zknyy/tmx-licensing-service/

重新build的 Dockerfile已经做了更新，使用了自己创建的docker web服务来下载jce库文件(原地址已经无法下载)，其中 "http://67.209.185.207:32768/jce_policy-8.zip" 就是临时的解决方法（也许直接用ADD更简单）；
其docker web服务的Dockerfile，和jce库文件都在Enhanced目录下
注意：zknyy/tmx-licensing-service:chapter3 只能对应 dev 环境（如果有时间再做成chapter3-dev  和   chapter3-prod 形式来区分）
      
##2. 修改licensing-service项目：原因是 license服务中的文件 bootstrap.yml  也必须是dev， 然后在docker build

##3. 对于数据库密码加密：
JCE安装目录 拷贝两个jar文件到（MacOS目录结构） 
**/Library/Java/JavaVirtualMachines/\*\*< jdk_version_of_your_pc >\*\*/Contents/Home/jre/lib/security**
改动java.security文件，找到  #crypto.policy=unlimited，并去掉注释即可。

##4. 另外补充：本地通过命令行参数覆盖启动App
**java -Dspring.cloud.config.uri=http://localhost:8888 -Dspring.profiles.active=dev -jar target/licensing-service-0.0.1-SNAPSHOT.jar**

----------------------------------------------------------------

# Introduction
Welcome to Spring Microservices in Action, Chapter 3.  Chapter 3 introduces the Spring Cloud Config service and how you can use it managed the configuration of your microservices.  By the time you are done reading this chapter you will have built and/or deployed:

1.  A Spring Cloud Config server that is deployed as Docker container and can manage a services configuration information using a file system or GitHub-based repository.
2.  A organization service that will manage organization data used within EagleEye.
3.  A licensing service that will manage licensing data used within EagleEye.
4.  A Postgres SQL database used to hold the data for these two services.

# Software needed
1.	Apache Maven (http://maven.apache.org). I used version 3.3.9 of the Maven. I chose Maven because, while other build tools like Gradle are extremely popular, Maven is still the pre-dominate build tool in use in the Java ecosystem. All of the code examples in this book have been compiled with Java version 1.8.
2.	Docker (http://docker.com). I built the code examples in this book using Docker V1.12 and above. I am taking advantage of the embedded DNS server in Docker that came out in release V1.11. New Docker releases are constantly coming out so it's release version you are using may change on a regular basis.
3.	Git Client (http://git-scm.com). All of the source code for this book is stored in a GitHub repository. For the book, I used version 2.8.4 of the git client.

# Building the Docker Images for Chapter 3
To build the code examples for Chapter 3 as a docker image, open a command-line window change to the directory where you have downloaded the chapter 3 source code.

Run the following maven command.  This command will execute the [Spotify docker plugin](https://github.com/spotify/docker-maven-plugin) defined in the pom.xml file.  

   **mvn clean package docker:build**

This is the first chapter we will have multiple Spring projects that need to be be built and compiled.  Running the above command at the root of the project directory will build all of the projects.  If everything builds successfully you should see a message indicating that the build was successful.

# Running the services in Chapter 3

Now we are going to use docker-compose to start the actual image.  To start the docker image,
change to the directory containing  your chapter 3 source code.  Issue the following docker-compose command:

   **docker-compose -f docker/common/docker-compose.yml up**

If everything starts correctly you should see a bunch of Spring Boot information fly by on standard out.  At this point all of the services needed for the chapter code examples will be running.
