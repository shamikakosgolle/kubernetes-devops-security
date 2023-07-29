pipeline {
  agent any

  stages {
    stage('Build Artifact - Maven') {
      steps {
        sh "mvn clean package -DskipTests=true"
        archive 'target/*.jar'
      }
    }

    stage('Unit Tests - JUnit and Jacoco') {
      steps {
        sh "mvn test"
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
          jacoco execPattern: 'target/jacoco.exec'
        }
      }
    }
       stage('Mutation Tests - PIT') {
      steps {
        sh "mvn org.pitest:pitest-maven:mutationCoverage"
      }
      post {
        always {
          pitmutation mutationStatsFile: '**/target/pit-reports/**/mutations.xml'
        }
      }
    }

    stage('Sonar Cube') {
      steps {
        sh " mvn sonar:sonar -Dsonar.projectKey=sonarpipe -Dsonar.host.url=http://104.131.177.241:9000 -Dsonar.login=fbe507d14f68d1051f557c6899c2c7f04a0d97c3"
      }
    }

   

    stage('Docker Build and Push') {
      steps {
        withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
          sh 'printenv'
          sh 'docker build -t shamikakosgolle/madu:""$GIT_COMMIT"" .'
          sh 'docker push shamikakosgolle/madu:""$GIT_COMMIT""'
        }
      }
    }
 
        stage('Kubernetes Deployment - DEV') {
      steps {
        withKubeConfig([credentialsId: 'kube']) {
          sh "sed -i 's#replace#shamikakosgolle/madu:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
          sh "kubectl apply -f k8s_deployment_service.yaml"
  }
}
        }
  }
}
