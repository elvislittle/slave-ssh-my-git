pipeline {
    agent { label 'jenkins-slave-label' }
    tools {
      maven 'Maven-3.9.4'
    }
    stages {
        stage('Source') {
            steps {
                sh 'mvn --version'
                sh 'git --version'
                git branch: 'main',
                    url: 'https://github.com/elvislittle/ssh-agent.git'
            }
        }
        stage('Clean') {
            steps {
                dir("${env.WORKSPACE}"){
                    sh 'mvn clean'
                }
            }
        }
        stage('Test') {
            steps {
                dir("${env.WORKSPACE}"){
                    sh 'mvn test'
                }
            }
        }
        stage('Package') {
            steps {
                dir("${env.WORKSPACE}"){
                    sh 'mvn package -DskipTests'
                }
            }
        }

        stage('Deliver to Master EC2') {
            steps {
                script {
                    def remoteUser = 'ubuntu'
                    def remoteHost = 'ec2-13-53-98-65.eu-north-1.compute.amazonaws.com'
                    def remoteDir = '~/'
                    sh "scp -i ~/SSH_keys/jenkins-master.pem ${env.WORKSPACE}/target/JavaDummy.jar ${remoteUser}@${remoteHost}:${remoteDir}/"
                }
            }
        }
    }
}
