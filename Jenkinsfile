pipeline {
    agent any
    triggers {
        githubPush() // GitHub에 커밋이 발생하면 자동으로 파이프라인 시작
    }
    environment {
        REPOSITORY = "donggu"
        DOCKERHUB_CREDENTIALS = credentials('docker-token')
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
                      def image = docker.build('doron9/rapa:k8smysql', '.')
                    }
                    dir('nginx/'){
                      def image = docker.build('doron9/rapa:k8snginx'', '.')
                    }
                    dir('tomcat/'){
                      def image = docker.build('doron9/rapa:k8stomcat'', '.')
                    }
                }
            }
        }
        stage('Docker Login') {
            steps {
                script {
                    def credentials = credentials('docker-token')
                    sh "echo \$DOCKERHUB_CREDENTIALS_PSW | docker login -u \$DOCKERHUB_CREDENTIALS_USR --password-stdin"
                }
            }
        }
        stage('Docker Push') {
            steps {
                script {
                    sh "docker push dorong9/rapa:k8smysql"
                    sh "docker push dorong9/rapa:k8snginx"
                    sh "docker push dorong9/rapa:k8stomcat"
                }
            }
        }
    }
}
