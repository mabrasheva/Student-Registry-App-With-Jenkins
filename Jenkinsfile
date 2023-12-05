pipeline {
    agent any

    stages {
    //     stage("Repo checkout") {
    //         steps {
    //             checkout scm
    // }
    //     }

        stage('NPM Install') {
            steps {
                bat "npm install"
            }
   }

        stage('Run integration tests') {
            steps {
                bat "npm run test"
            }
        }

        stage('Build Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: '2770e446-5cfe-423d-91c0-1f1ffa305476', passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
                    bat """docker build -t mariaabr/student-registry-app:1.0.0 .
                        docker login -u %DOCKER_USERNAME% --password %DOCKER_PASSWORD%
                        docker push mariaabr/student-registry-app:1.0.0"""
                }
            }
        }
        
        stage('Deploy Image') {
            steps {
              script {
                    // Prompt for input approval
                    input("Deploy to production?") 
                }
                withCredentials([usernamePassword(credentialsId: '2770e446-5cfe-423d-91c0-1f1ffa305476', passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
                    bat """docker pull mariaabr/student-registry-app:1.0.0
                    docker run -d -p 8080:8080 mariaabr/student-registry-app:1.0.0 """
                }
            }
        }
    }
}