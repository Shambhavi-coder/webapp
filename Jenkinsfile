pipeline {
    agent {
        label 'Slave01'
    }


    
    stages {

        stage('Build') {
            steps {
                bat 'mvn -B -DskipTests clean package'
            }
        }

        stage('Test') {
            steps {
                bat 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Sonar-Report') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat 'mvn sonar:sonar'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    if (env.BRANCH_NAME == "develop") {
                        bat 'start /B java -DappPort=9999 -jar target/java-webapp-1.0-shaded.jar'
                    } else if (env.BRANCH_NAME == "feature") {
                        bat 'start /B java -DappPort=9997 -jar target/java-webapp-1.0-shaded.jar'
                    }
                }
            }
        }
    }
}
