# 쿠버네티스 클러스터 환경에서 3티어 구성하기

1. namespace 생성
	namespace 폴더에 들어가서 kubectl apply -f .
	-> metallb와, donggu-ns 생성
	  * metallb 가 있다면 kubectl apply -f donggu-ns.yaml 

2. ingress 생성
	* 인그레스 컨트롤러 설치 후 진행 
	my-ingress.yaml 실행
	/etc/hosts 에 '211.18.3.xxx  www.donggu.pri' 추가

3. DB 생성
	mysql-deploy.yaml 실행

4. WAS 생성
   1. 만약 소스코드를 변경하고 싶다면
	java JDK 17 이상 설치
	Maven 최신버전 설치
	환경변수 지정 
		/etc/environment 에 JAVA_HOME="/usr/lib/jvm/java-17-openjdk-amd64" 추가
	
	cd spring-framework-petclinic
	./mvnw clean package -Dmaven.test.skip=true -P MySQL

	target/petclinic.war 파일 생성

	petclinic.war 파일을 tomcat 폴더에 복사 후 도커 이미지 생성 
   
   2. tomcat 폴더에서 kubectl apply -f tomcat-deploy.yaml 

5. WEB 생성
	web 폴더에서 kubectl apply -f nginx-deploy.yaml

6. 결과확인
	kvm 내 www.donggu.pri/petclinic/
	윈도우 웹에서 211.183.3.xxx:30010/petclinic/
