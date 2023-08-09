pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
    }

    stages {
        stage('Prepare') {
            steps {
                // Get some code from a GitHub repository
                git branch: 'main',
                    url: ''
            }

            post {
                success {
                    echo "Preparation is successfull ! :)"
                }
            }
        }

        stage('Build'){
            steps{
                sh mvn compile
            }

            post{
                success{
                echo "Build is succesfull ! :)"
                }
            }
        }

        stage('Test'){
            steps{                
                    sh "mvn -Dmaven.test.failure.ignore=true clean package"
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.war'
            }
            post{
                success{
                echo "Test is done" 
                }
            }
        }

        stage("Email"){
            steps{
                echo 'Sending email notification ...'
                mail to: 'ok22kor@bosch.com',
                subject: "Pipeline build is successfull",
                body: "Test email notification for pipeline"

            }
            post{
                success{
                    echo "Email sent !!"
                }
            }
        }

    }
}
