pipeline {
  agent any

  tools {
    maven 'M2_HOME'
    
    }
  
  stages {
   stage('CheckOut') {
      steps {
        echo 'Checkout the source code from GitHub'
        git 'https://github.com/roshanscgithub/InsureMe-project.git'
            }
    }
    
    stage('Package the Application') {
      steps {
        echo " Packaing the Application"
        sh 'mvn clean package'
            }
    }
    
    stage('Publish TestNG Reports using HTML') {
      steps {
      publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '/root/.jenkins/workspace/Banking-project/target/surefire-reports', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])
            }
    }
    
    stage('Docker Image Creation') {
      steps {
        sh 'docker build -t roshdockerhub/banking-project:1.0' 
            }
    }
    stage('DockerLogin') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'Dockerlogin', passwordVariable: 'dockerhub-pass', usernameVariable: 'dockerhub-user')]) {
        sh 'docker login -u ${env.dockerhub-user} -p ${env.docker-pass}'
            }
        }
    } 
  
    stage('Push Image to DockerHub') {
      steps {
        sh 'docker push roshdockerhub/banking-project:1.0'
            }
    } 
  
  
  }
}
