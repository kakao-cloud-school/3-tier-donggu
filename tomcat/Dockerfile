# 베이스 이미지 설정
FROM tomcat:10-jre17

# 필요한 패키지 설치
RUN apt-get update && \
    apt-get install -y openjdk-17-jdk maven git && \
    rm -rf /var/lib/apt/lists/*

# 소스 코드 클론
RUN git clone https://github.com/spring-petclinic/spring-framework-petclinic.git /root/spring-framework-petclinic

# pom.xml 수정
RUN sed -i '517s#.*#                <jdbc.url>jdbc:mysql://mysql-petclinic:3306/petclinic?useUnicode=true</jdbc.url>#' /root/spring-framework-petclinic/pom.xml && \
    sed -i '518s#.*#                <jdbc.username>root</jdbc.username>#' /root/spring-framework-petclinic/pom.xml && \
    sed -i '519s#.*#                <jdbc.password>test123</jdbc.password>#' /root/spring-framework-petclinic/pom.xml

# 소스 코드 빌드 및 WAR 파일 복사
RUN cd /root/spring-framework-petclinic && \
    ./mvnw clean package -Dmaven.test.skip=true -P MySQL && \
    cp /root/spring-framework-petclinic/target/petclinic.war /usr/local/tomcat/webapps/
