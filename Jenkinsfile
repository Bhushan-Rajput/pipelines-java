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
                git branch: 'main',
                        url: 'https://github.com/Bhushan-Rajput/pipelines-java.git'

            }   
        }
        
        
        stage('Build'){
            steps {
                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
                
            }
            
        }
        

        stage('Deploy'){
            steps {
                echo "Deploy step is added"
                docker adapters: [tomcat9( 
                    credentialId: 'Tomcat-Deployment',
                    path: '',
                    url: 'http://172.190.107.213:8088'
                )]
                contextPath: 'servletproject',
                onfailure: 'false',
                wwar: '**/*.war'
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