pipeline{
    agent any
    environment{
        SONAR_HOME= tool "Sonar"
    }
    stages{
        stage("Clone Code from Github"){
            steps{
                git url: "https://github.com/hinaakram027/Nodejs-application", branch: "main"
            }
        }
        stage("SonarQube Quality Analysis"){
            steps{
                withSonarQubeEnv("Sonar"){
                    sh "$SONAR_HOME/bin/sonar-scanner -Dsonar.projectName=Nodejs-application -Dsonar.projectKey=Nodejs-application"
                }
            }
        }
        stage("Trivy File Scan"){
            steps{
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
        }
        stage("Build Docker Image"){
            steps{
                sh 'docker build -t hinaar/devsecops-1.1 .'
            }
        }
        stage("Push Docker Hub"){
            steps{
                withCredentials([string(credentialsId: 'hinaardocker', variable: 'hinaardocker')]) {
                    sh 'docker login -u hinaar -p ${hinaardocker}'
                }
                sh 'docker push hinaar/devsecops-1.1'
            }
        }
    }
}
