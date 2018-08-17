pipeline {
    agent any

    tools {
        maven 'localMaven'
        jdk 'localJDK'
    }

    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage ('Deploy to Staging'){
            steps {
                sh "mvn war:war deploy:deploy"
                build job: 'deploy-to-staging'
            }
        }
        stage ('Deploy to Prod'){
            steps {
                timeout(time:5, unit:'DAYS'){
                    input message:'Approve PRODUCTION Deployment?'
                }
                sh "mvn war:war deploy:deploy"
                build job: 'deploy-to-prod'
            }
            post {
                success {
                    echo 'Code deployed to Production.'
                }

                failure {
                    echo ' Deployment failed.'
                }
            }
        }
    }
}
