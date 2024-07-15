#!/usr/bin/env groovy

library identifier: 'jenkins-shared-library@main', retriever: modernSCM(
    [$class: 'GitSCMSource',
     remote: 'https://github.com/ibrahim-osama-amin/Jenkins-shared-library.git',
     credentialsId: 'github-credentials'
    ]
)

pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    environment {
        IMAGE_NAME = 'ibrahimosama/my-repo:ec2-image'
    }
    stages {
        stage('build app') {
            steps {
               script {
                  echo 'building application jar...'
                  buildJar()
               }
            }
        }
        stage('build image') {
            steps {
                script {
                   echo 'building docker image...'
                   buildImage(env.IMAGE_NAME)
                   dockerLogin()
                   dockerPush(env.IMAGE_NAME)
                }
            }
        }
        stage ('User input of EC2 public IP'){
            steps {
                script {
                    def userInput = input id: 'userInput', message: 'Please enter the EC2 public IP address:', parameters: [string(name: 'PUBLIC_IP', defaultValue: '', description: 'EC2 Public IP Address')]
                    echo "Captured user input: ${userInput}"
                    env.PUBLIC_IP = userInput.trim()
                    
                    echo "Assigned PUBLIC_IP: ${env.PUBLIC_IP}"
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                   echo 'deploying docker image to EC2...'
                   echo "Using EC2 public IP: ${env.PUBLIC_IP}"

                   def shellCmd = "bash ./server-cmds.sh ${IMAGE_NAME}"
                   def ec2Instance = "ec2-user@${env.PUBLIC_IP}"
                   echo "EC2 instance: ${ec2Instance}"

                   sshagent(['ec2-server-key-paris']) {
                       sh "scp -o StrictHostKeyChecking=no server-cmds.sh ${ec2Instance}:/home/ec2-user"
                       sh "scp -o StrictHostKeyChecking=no docker-compose.yaml ${ec2Instance}:/home/ec2-user"
                       sh "ssh -o StrictHostKeyChecking=no ${ec2Instance} ${shellCmd}"
                   }
                }
            }
        }
    }
}
