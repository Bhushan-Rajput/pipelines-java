pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
    }

    stages {
         
        stage('Checkout') {
            
            steps {
                // Get some code from a GitHub repository
                echo "Checkout Stage"
                git branch: 'main',
                    url: 'https://github.com/Bhushan-Rajput/pipelines-java.git'

             }
        }

        stage ('Build') {

            steps {
                echo "Build Stage"
                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
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
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.war'
                }
            }
        }
    }
}
