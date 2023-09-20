pipeline {
    agent any

    tools {
        maven "M3"
    }

    stages {
         
        stage('Checkout') {
            
            steps {
                echo "Checkout Stage"
                checkout scm

             }
        }

        stage ('Build') {

            steps {
                echo "Build Stage is updated"
                
                sh "mvn -Dmaven.test.failure.ignore=true compile test"

                
            }

        }
        stage('Install'){
            when {
                branch 'develop'
            }
            steps {
                sh 'mvn install'
            }
        }

        stage ('Deploy') {
            when {
                branch 'main'
            }

            steps {
                echo "Deploy Stage"
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
                deploy adapters: [tomcat9 (
                        credentialsId: 'Tomcat_admin',
                        path: '',
                        url: 'http://52.146.7.110:8088/'
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

        // stage ('SonarQube Metrics') {
        //     steps {
        //         sh "mvn clean verify sonar:sonar   -Dsonar.projectKey=test1   -Dsonar.host.url=http://20.55.6.203:9000   -Dsonar.login=sqp_62c62ae2100e0e256bef53895692b7551b6c6530"
        //     }
        // }
    }
}
