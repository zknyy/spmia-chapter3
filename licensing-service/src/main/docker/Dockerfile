FROM openjdk:8-jdk-alpine
RUN  apk update && apk upgrade && apk add netcat-openbsd
RUN  apk add curl
RUN  curl –k-LO "http://67.209.185.207:32768/jce_policy-8.zip" > /jce_policy-8.zip 
RUN  unzip /jce_policy-8.zip 
RUN  rm /jce_policy-8.zip
RUN  ls -lasth /
RUN  yes |cp -v /UnlimitedJCEPolicyJDK8/*.jar /usr/lib/jvm/java-1.8-openjdk/jre/lib/security/
#RUN  cp /tmp/UnlimitedJCEPolicyJDK8/local_policy.jar /usr/lib/jvm/java-1.8-openjdk/jre/lib/security/
#RUN  cp /tmp/UnlimitedJCEPolicyJDK8/US_export_policy.jar /usr/lib/jvm/java-1.8-openjdk/jre/lib/security/
RUN mkdir -p /usr/local/licensingservice
ADD @project.build.finalName@.jar /usr/local/licensingservice/
ADD run.sh run.sh
RUN chmod +x run.sh
CMD ./run.sh
