pipeline {
    agent { label 'jenkins-slave-label' }
    tools {
      maven 'Maven-3.9.4'
    }
    stages {
        stage('Getting source code') {
            steps {
                sh 'mvn --version'
                sh 'git --version'
                git branch: 'main',
                    url: 'https://github.com/elvislittle/ssh-agent.git'
            }
        }
        stage('Cleaning environment') {
            steps {
                dir("${env.WORKSPACE}"){
                    sh 'mvn clean'
                }
            }
        }
        stage('Testing the app') {
            steps {
                dir("${env.WORKSPACE}"){
                    sh 'mvn test'
                }
            }
        }
        stage('Packing the app') {
            steps {
                dir("${env.WORKSPACE}"){
                    sh 'mvn package -DskipTests'
                }
            }
        }

        stage('Delivering the app to Master EC2') {
            steps {
                script {
                    def remoteUser = 'ubuntu'
                    def remoteHost = 'ec2-13-53-98-65.eu-north-1.compute.amazonaws.com'
                    def remoteDir = '~/'
                    sh "scp -i ~/SSH_keys/jenkins-master.pem ${env.WORKSPACE}/target/Goal-1.0.jar ${remoteUser}@${remoteHost}:${remoteDir}/"
                }
            }
        }
    }
}
