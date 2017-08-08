pipeline {
  agent any

  stages{ 
    stage ('Unit Tests') {
	steps {
           sh 'ant -f test.xml -v'
           junit 'reports/result.xml'
       }
    } 
    stage('Build') {
      steps {
        sh 'ant -f build.xml -v'
      }
    }
    stage('Deploy') {
    agent {
        label 'apache'
    }
      steps {
        #sh "if ![ -d '/var/www/html/rectangles/all/${env.BRANCH_NAME}' ]; then mkdir /var/www/html/rectangles/all/${env.BRANCH_NAME}; fi"
        sh "cp dist/rectangle_.${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/"
      }
    }
  }

  post{
    always {
	  archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
	}
  }
}
