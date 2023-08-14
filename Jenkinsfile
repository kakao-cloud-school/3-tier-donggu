pipeline {
    agent any
    triggers {
        githubPush() // GitHub에 커밋이 발생하면 자동으로 파이프라인 시작
    }
    environment {
        REPOSITORY = "innnnnwoo"
        DOCKERHUB_CREDENTIALS = credentials('docker_accesstkn')
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/kakao-cloud-school/3-tier-donggu.git'
            }
        }
        stage('Build') {
            steps {
                script {
                    dir('mysql/'){
                      def image = docker.build('innnnnwoo/d-mysql', '.')
                    }
                    dir('nginx/'){
                      def image = docker.build('innnnnwoo/d-nginx', '.')
                    }
                    dir('tomcat/'){
                      def image = docker.build('innnnnwoo/d-tomcat', '.')
                    }
                }
            }
        }
        stage('Docker Login') {
            steps {
                script {
                    def credentials = credentials('docker_accesstkn')
                    sh "echo \$DOCKERHUB_CREDENTIALS_PSW | docker login -u \$DOCKERHUB_CREDENTIALS_USR --password-stdin"
                }
            }
        }
        stage('Docker Push') {
            steps {
                script {
                    sh "docker push innnnnwoo/d-mysql"
                    sh "docker push innnnnwoo/d-nginx"
                    sh "docker push innnnnwoo/d-tomcat"
                }
            }
        }
    }
}
