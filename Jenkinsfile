pipeline {
    agent any

    triggers {
        pollSCM('H/2 * * * *')
    }

    stages {
        stage('Get code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker image') {
            steps {
                sh 'docker build -t html-demo:${BUILD_NUMBER} .'
            }
        }

        stage('Deploy with Ansible') {
            steps {
                sh '''
                    ansible-playbook -i 'localhost,' ansible/deploy.yml \
                      -e image_tag=${BUILD_NUMBER}
                '''
            }
        }

        stage('Check website') {
            steps {
                sh 'curl --fail http://localhost/'
            }
        }
    }
}