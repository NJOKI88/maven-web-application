pipeline {
  agent {
    label 'agent4'
  }
  
  environment {
    MAVEN_HOME = tool name: 'Maven3.9.2'
  }
  
  stages{
    stage('1.GetCode') {
      steps {
        git "https://github.com/NJOKI88/maven-web-application"
        //sh "git clone https://github.com/NJOKI88/maven-web-application"
        //bat "git clone https://github.com/NJOKI88/maven-web-application"
      }
    }
    
    stage('2.Build') {
      steps {
        sh "${env.MAVEN_HOME}/bin/mvn package"
      }
    }
    
    stage('3.codeQualityAnalysis') {
      steps {
        sh "${env.MAVEN_HOME}/bin/mvn sonar:sonar"
      }
    }
    
    stage('4.upload') {
      steps {
        sh "${env.MAVEN_HOME}/bin/mvn deploy"
      }
    }
    
    stage('5.deploy2UAT') {
      steps {
        deploy adapters: [tomcat9(credentialsId: 'tomcat-credentials', path: '', url: 'http://3.91.182.181:8080/')], contextPath: null, war: 'target/*war'
      }
    }
    
    stage('6.Approval') {
      steps {
        echo 'Apps ready for review'
        timeout(time: 5, unit: 'HOURS') {
          input message: 'Application ready for deployment. Please review and approve.'
        }
      }
    }
  }
}
