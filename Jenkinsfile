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
        stage('Maven'){
            steps {
                sh "mvn -Dmaven.test.failure.ignore=true clean validate compile test package"
            }
        }
        stage('Docker Build'){
            steps {
                sh 'docker build -t aswin615/spring-petclinic .'
            }
        }
        stage('Docker push'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPass', usernameVariable: 'dockerHubUser')]) {
                    sh 'docker login -u ${dockerHubUser} -p ${dockerHubPass}'
                    sh 'docker push aswin615/spring-petclinic'
                }
            }
        }
        stage('Docker run'){
            steps{
                sh 'docker run -d --name petclinic -p 8090:8080 aswin615/spring-petclinic'
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
