pipeline {
    agent {
        label 'Slave01'
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/Shambhavi-coder/webapp'
            }
        }

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
                bat '''
                echo ===== DEPLOYING APPLICATION =====

                REM kill process on port 9999 if running
                for /f "tokens=5" %%a in ('netstat -aon ^| findstr :9999') do taskkill /PID %%a /F

                REM start application
                start /B java -DappPort=9999 -jar target\\java-webapp-1.0-shaded.jar

                echo ===== DEPLOYMENT DONE =====
                '''
            }
        }
    }
}
