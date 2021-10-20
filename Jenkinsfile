pipeline {
  agent {
    label 'agent'
  }
    tools{
       maven 'maven'
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
    }
}
