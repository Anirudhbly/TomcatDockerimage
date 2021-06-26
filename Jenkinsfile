pipeline {
     environment {
     registry = "anirudhbly/anirudh_tomcat"
     registryCredential = 'dockerhub_id'
     dockerImage = ''
     }
     agent any
     stages {
          stage("Compile") {
               steps {
                    sh "/usr/bin/mvn compile"
               }
          }
          stage("Unit test") {
               steps {
                    sh "/usr/bin/mvn test"
               }
                           post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
          }
     
    
stage("Package") {
     steps {
          sh "/usr/bin/mvn package"
     }
}
stage("Docker build") {
     steps {
      
          sh "docker build . -t anirudh_tomcat"
     }
}

stage("Deploy to staging") {
     steps {
          
          sh "docker stop \$(docker ps -qa)"
          sh "docker rm \$(docker ps -qa)"
          sh "docker run -d -it -v /var/lib/jenkins/workspace/Demo_Docker/target/:/usr/local/tomcat/webapps/ -p 8090:8080 --name Testtomcat anirudh_tomcat"
     }
}
          
stage('Building our image') {
      steps{
      script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
}
}
}
stage('Push the image to Dockerhub') {
      steps{
      script {
      docker.withRegistry( '', registryCredential ) {
      dockerImage.push()
}
}
}
}

     }
  post {
     always {
          sh "echo 'I did It'"
     }
}
}
