pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: 'localhost', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'localhost', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

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
/*
        stage ('Deploy to Staging'){
            steps {
                build job: 'deploy-to-staging'
            }
        }
        stage ('Deploy to Prod'){
            steps {
                timeout(time:5, unit:'DAYS'){
                    input message:'Approve PRODUCTION Deployment?'
                }
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
*/
        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "pwd"
                        sh "cp **/target/*.war /Users/Shared/Jenkins/Home/workspace/deploy-to-staging/webapp/"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "cp **/target/*.war /Users/Shared/Jenkins/Home/workspace/deploy-to-prod/webapp/target/"
                    }
                }
            }
        }

    }
}
