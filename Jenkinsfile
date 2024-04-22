pipeline {
    environment {
        dockerimagename = 'taskmanagerapp'
        dockerimage = ''
    }
    agent any

    tools {
        git 'Default'
    }

    stages {
        stage('Code Source') {
            steps {
                // Pull code from the Task-Manager repository
                checkout([$class: 'GitSCM', branches: [[name: '*']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/Takapiwanashe/task-manager.git']]])
            }
        }
// [[name: '*/main']]
        stage('Image Build') {
            steps {
                script {

                    dockerimage = docker.build dockerimagename
                }
            }
        }

        stage('Task-Manager Deployment') {
            steps {
                // Pull images and deploy using kubectl
                sh 'kubectl apply -f web-deployment.yml'
                sh 'kubectl apply -f postgres-deployment.yml'
                sh 'kubectl apply -f postgres-configmap.yml'
                sh 'kubectl apply -f postgres-secrets.yml'
            }
        }
    }
}
