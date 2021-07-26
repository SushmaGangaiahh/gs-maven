pipeline {
    agent any
    tools {
        maven "MAVEN_HOME"
    }

    stages {
        stage('scm checkout') {
            steps {
                shell '''
                mkdir gs-maven
                cd gs-maven
                pwd
                '''
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'Git_cred', url: 'https://github.com/SushmaGangaiahh/gs-maven.git']]])
            }
        }
        stage('Build') {
            steps {
                sh '''
                pwd
                cd initial
                pwd
                mvn -Dmaven.test.failure.ignore=true clean package
                '''
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
    }
}
