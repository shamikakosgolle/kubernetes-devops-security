pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' //so that they can be dowyigiygigicciogi
            }
        }
          stage('junit test Artifact') {
            steps {
              sh "mvn test"
                 post {
        always {
          junit 'target/surefirekosa-reports/*.xml'
         
        }
            }
        } 
    }
}
}
