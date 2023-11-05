pipeline {
    agent any
    tools {
        maven 'Maven_local'
    }
   
    stages {
        stage ("Code") {
            steps {
               git url: "https://github.com/darshanchavan1211/java-app-repo.git" , branch: "main" 
            }
            
            
        }
        stage('Build') {
            steps {
                echo 'Building..'
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
                dir("${env.WORKSPACE}/java-tomcat-sample"){
                    sh "pwd"
                    sh 'mvn clean install -f pom.xml'
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                
            }
        }
        stage("Docker build") {
            steps {
                sh "docker build -t java-app ."
            }
        }
        stage("Push to ECR"){
            steps {
                script {
                    sh "aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 356417641411.dkr.ecr.ap-south-1.amazonaws.com"
                    sh "aws ecr create-repository --repository-name java-app --region ap-south-1"
                    sh "docker tag java-app:latest 356417641411.dkr.ecr.ap-south-1.amazonaws.com/java-app:latest"
                    sh "docker push 356417641411.dkr.ecr.ap-south-1.amazonaws.com/java-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps {
                sh "docker run -d -p 80:80 --name java-container 356417641411.dkr.ecr.ap-south-1.amazonaws.com/java-app:latest"
            }
        }
    }
}










