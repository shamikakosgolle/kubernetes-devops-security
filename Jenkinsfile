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

    stage('Docker Build and Push') {
      steps {
        withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
          sh 'printenv'
          sh 'docker build -t shamikakosgolle/kosa:""$GIT_COMMIT"" .'
          sh 'docker push shamikakosgolle/kosa:""$GIT_COMMIT""'
        }
      }
    }

        stage('Kubernetes Deployment - DEV') {
      steps {
        withKubeConfig([credentialsId: 'kube']) {
          sh "sed -i 's#replace#shamikakosgolle/kosa:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
          sh "kubectl apply -f k8s_deployment_service.yaml"
  }
}
