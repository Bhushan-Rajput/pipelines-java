pipeline {
    agent any

    tools {
        maven "M3"
    }

    stages {
         
        stage('Checkout') {
            
            steps {
                echo "Checkout Stage"
                git branch: 'main',
                    url: 'https://github.com/Bhushan-Rajput/pipelines-java.git'

             }
        }

        stage ('Build') {

            steps {
                echo "Build Stage is updated"
                
                sh "mvn -Dmaven.test.failure.ignore=true clean package"

                
            }

        }

        stage ('Deploy') {

            steps {
                echo "Deploy Stage"
                deploy adapters: [tomcat9 (
                        credentialsId: 'Tomcat_admin',
                        path: '',
                        url: 'http://20.42.118.253:8088'
                    )],
                    contextPath: 'question3',
                    onFailure: 'false',
                    war: '**/*.war'
            }
            post {
                
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.war'
                }
            }
        }
    }
}
