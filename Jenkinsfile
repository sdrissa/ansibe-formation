pipeline {
    agent any

    stages {
        stage('Build artifact'){
            steps{
                sh 'mvn clean package -DskipTest=true'
            }
        }
          stage('Test artifact'){
            steps{
                sh 'mvn test'
            }
        }
              stage('Build and psh image'){
            steps{
                withDockerRegistry([credentialsId: "242b355f-b71f-4ab8-9a4e-b191b0aca71b", url: ""]) {
                    sh 'docker build -t dorsis/demo-bceao:$BUILD_NUMBER .'
                    sh 'docker push dorsis/demo-bceao:$BUILD_NUMBER'
                }
            }
        }
           stage('run image'){
            steps{
                 sh 'docker stop demo-boot || true'
                 sh 'docker rm demo-boot || true'
                 sh 'docker run -d -p 8180:8180 --name demo-boot dorsis/demo-bceao:$BUILD_NUMBER'
            }
        }
    }
}
