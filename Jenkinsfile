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
        sh "/usr/local/bin/docker build . -t tomcatwebapp:${env.BUILD_ID} -v docker:/usr/local/bin/docker -v /var/run/docker.sock:/var/run/docker.sock "
      }
    }
  }
}
