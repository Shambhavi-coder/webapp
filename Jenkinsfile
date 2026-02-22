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
//         stage('Sonar-Report') {
//             steps {
//             sh 'mvn sonar:sonar \
//   -Dsonar.projectKey=jenkins_project \
//   -Dsonar.host.url=http://localhost:9000 \
//   -Dsonar.login=5f09ded7e5db4d0ea0dcfd937c181af706e60475'
//             }
//         }
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
      //  stage('Sonar-Report') {
        //    steps {
          //      bat 'mvn clean install sonar:sonar -Dsonar.host.url=http://localhost:9000 -Dsonar.analysis.mode=publish'
            //}
        //}
    stage('Deploy') {
    steps {
        script {
            if (env.BRANCH_NAME == "develop") {
                bat 'set PORT=9999 && start /B java -jar target/java-webapp-1.0-shaded.jar'
            }
            else if (env.BRANCH_NAME == "feature") {
                bat 'set PORT=9997 && start /B java -jar target/java-webapp-1.0-shaded.jar'
            }
        }
    }
}
    }
}
    }
}
