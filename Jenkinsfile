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
      stage('deploy'){
          steps{
              sshagent(['deploy_user1']) {
                sh "scp -o StrictHostKeyChecking=no /home/jenkins/agent/workspace/shreya-training/mav_app_pipe/target/MavApp.war ec2-user@3.235.11.169:/opt/apache-tomcat-8.5.71/webapps"
              }
          }
      }
    }
}
