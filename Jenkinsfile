pipeline {
  agent {
    label 'agent'
  }
    tools{
       maven 'maven'
    }
  environment {
        CI = 'true'
        registry = 'docker-services-training/shreya/'
    }

    stages {
        stage('scm') {
            steps {
                git branch:'master',url:'https://github.com/shreyadey16/MavAppRepo.git'
            }
        }
        stage('Build') {
            steps {
                sh ' mvn clean package'
                stash name: "artifact"
            }
        }
      stage('SonarQube Analysis'){
            steps{
                script{
                    withSonarQubeEnv('sonar'){
                        sh 'ls'
                        sh 'mvn sonar:sonar -DskipTests'
                    }
                }
            }
        }
      stage('Build the image') { 
            steps { 
                script {
                    unstash name:"artifact"
                    docker.withTool('docker') {
                        docker.withRegistry('https://artifactory.dagility.com', 'shreya-registry'){
                            docker.build(registry + "samp_maven:latest").push()
                        }
                    }
                } 
            }
         }
      
    }
}
