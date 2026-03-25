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
                bat '''
                echo ===== DEPLOYING APPLICATION =====

                cd %WORKSPACE%

                dir target

                REM kill old process
                for /f "tokens=5" %%a in ('netstat -aon ^| findstr :9999') do taskkill /PID %%a /F

                REM find jar dynamically
                for %%f in (target\\*.jar) do set JAR=%%f

                echo Running %JAR%

                REM run jar
                start /B java -DappPort=9999 -jar %JAR%

                echo ===== DEPLOYMENT DONE =====
                '''
            }
        }
    }
}
