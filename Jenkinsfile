pipeline{
    agent any
    environment{
        SONAR_HOME= tool "Sonar"
    }
    stages{
        stage("Clone Code from Github"){
            steps{
                git url: "https://github.com/hinaakram027/two-tier-flask-app", branch: "main"
            }
        }
        stage("SonarQube Quality Analysis"){
            steps{
                withSonarQubeEnv("Sonar"){
                    sh "$SONAR_HOME/bin/sonar-scanner -Dsonar.projectName=two-tier-flask-app -Dsonar.projectKey=two-tier-flask-app"
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
                sh 'docker build -t hinaar/flaskappproject-1.1 .'
            }
        }
        stage("Push Docker Hub"){
            steps{
                withCredentials([string(credentialsId: 'hinaardocker', variable: 'hinaardocker')]) {
                    sh 'docker login -u hinaar -p ${hinaardocker}'
                }
                sh 'docker push hinaar/flaskappproject-1.1'
            }
        }
    }
}
