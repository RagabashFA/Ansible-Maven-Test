#!/usr/bin/env groovy

pipeline {
    agent any

    tools {
        // Install the Maven & JDK
        maven "maven3"
        jdk "JDK9"
    }

    stages {
        stage('Prepare') {
            steps {
                // Get some code from a GitHub repository
                git "https://github.com/RagabashFA/Jenkins-Ansible-Maven-Test.git"
            }
        }

        stage('Test Build') {
            steps {
                sh "mvn --batch-mode compile"
            }
        }

        stage('UnitTests') {
            steps {
               sh "mvn test"
            }
        
         post  {
            always {
               junit allowEmptyResults: true, testResults: 'target/surefire-reports/TEST-*.xml'
            }
         }
        }
        stage('Packaging') {
            steps {
                // Run Maven on a Unix agent.
                sh "mvn --batch-mode war:war"
            }
        }
        stage('Deploy') {
            steps {
                // Run Ansible playbook.
                sh "ansible-playbook ./site.yml --user ubuntu --key-file ~/.ssh/id_rsa.pub"
            }
        }
    }
}
