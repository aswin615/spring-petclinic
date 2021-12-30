pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
    }

    stages {
        stage('Source Code Checkout') {
            steps {
                // Get some code from a GitHub repository
                git branch: 'main', url: 'https://github.com/aswin615/spring-petclinic.git'
            }
        }
        stage('Maven-clean'){
            steps {
                sh "mvn -Dmaven.test.failure.ignore=true clean"
            }
        }
        stage('Maven-Vaidate'){
            steps('validate') {
                sh "mvn validate"
            }
        }
        stage('Maven-Compile'){
            steps('compile') {
                sh "mvn compile"
            }
        }
        stage('Maven-test'){
            steps('test') {
                sh "mvn test"
            }
        }
        stage('Maven-Package'){
            steps('Package') {
                sh "mvn package"
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
