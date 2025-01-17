pipeline {
    agent any
    
    tools {
        maven 'maven s/w'
    }
    stages {
        stage ('Git-Checkout') {
            steps {
                git "https://github.com/ashwiniboddu/Hello-World.git"
            }
        }
        stage ('Maven Build') {
            steps {
                sh "mvn clean package"
            }
        }
        stage ('Build Docker Image') {
            steps {
                sh "docker build -t hello-world:latest ."
                sh 'docker tag hello-world:latest bodduashwini/hello-world:latest'  // Ensure the correct tag
            }
        }
        stage ('Push Image to DockerHub') {
            steps {
                script {
                    withDockerRegistry(credentialsId: "dockerhub-cred") {
                        sh "docker push bodduashwini/hello-world:latest"
                    }
                }
            }
        }
        stage ('Deploy to Tomcat') {
            steps {
                script {
                    sh '''
                    docker rm -f tomcat || true
                    docker run -d --name tomcat -p 8081:8081 bodduashwini/hello-world:latest
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "Deployment is successfully completed"
        }
        failure {
            echo "Deployment is failed"
        }
    }
}