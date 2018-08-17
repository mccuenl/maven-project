pipeline {
  agent any

    tools {
        maven 'localMaven'
        jdk 'localJDK'
    }

  stages {
    stage('Build') {
      steps {
        sh 'mvn clean package'
        sh "/Applications/Docker.app/Contents/Resources/bin/docker build . -t tomcatwebapp:${env.BUILD_ID}"
      }
    }
  }
}
