pipeline {
  agent any

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
                bat '''
                echo ===== DEPLOYING APPLICATION =====

                cd %WORKSPACE%

                dir target

                REM kill old process
                for /f "tokens=5" %%a in ('netstat -aon ^| findstr :9999') do taskkill /PID %%a /F

                REM run correct jar (IMPORTANT FIX)
                start /B java -DappPort=9999 -jar target\\java-webapp-1.0.jar

                echo ===== DEPLOYMENT DONE =====
                '''
            }
        }
    }
}
