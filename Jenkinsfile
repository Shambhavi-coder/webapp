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
                    def branch = env.BRANCH_NAME ?: ""
                    echo "Branch: ${branch}"

                    if (branch.contains("develop")) {
                        bat '''
                        echo Deploying on port 9999...

                        REM kill existing process on port 9999
                        for /f "tokens=5" %%a in ('netstat -aon ^| findstr :9999') do taskkill /PID %%a /F

                        REM start application
                        start /B java -DappPort=9999 -jar target\\java-webapp-1.0-shaded.jar
                        '''
                    } else if (branch.contains("feature")) {
                        bat '''
                        echo Deploying on port 9997...

                        REM kill existing process on port 9997
                        for /f "tokens=5" %%a in ('netstat -aon ^| findstr :9997') do taskkill /PID %%a /F

                        REM start application
                        start /B java -DappPort=9997 -jar target\\java-webapp-1.0-shaded.jar
                        '''
                    } else {
                        echo "No deployment configured for this branch"
                    }
                }
            }
        }
    }
}
