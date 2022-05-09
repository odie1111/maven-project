pipeline {
    agent any
    parameters {
        string(name: 'staging_path', defaultValue: '/vagrant/apache-tomcat-8.5.75-staging')
        string(name: 'prod_path', defaultValue: '/vagrant/apache-tomcat-8.5.75-prod')
    }
    triggers {
        pollSCM('* * * * *')
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

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "cp  **/target/*.war ${params.staging_path}/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "cp **/target/*.war ${params.prod_path}/webapps"
                    }
                }
            }
        }
    }

}