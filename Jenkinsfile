pipeline {
  agent {
     label 'master'
  }

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
        sh "sudo cp dist/rectangle.jar /var/www/html/rectangles/all/"
      }
    }
  }

  post{
    always {
	  archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
	}
  }
   
 post{
  success {
    emailext(
            subject: "${env.JOB_NAME} [${env.BUILD_NUMBER}] Development Promoted to Master",
            body: """<p>'${env.JOB_NAME} [${env.BUILD_NUMBER}]' Development Promoted to Master":</p>
            <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
            to: "ly107h@att.com"
          )
         } 
 }

  post {
    failure {
      emailext(
        subject: "${env.JOB_NAME} [${env.BUILD_NUMBER}] Failed!",
        body: """<p>'${env.JOB_NAME} [${env.BUILD_NUMBER}]' Failed!":</p>
        <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
        to: "ly107h@att.com"
      )
    }
}


}
