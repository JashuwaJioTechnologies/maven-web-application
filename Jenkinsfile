pipeline { //pipeline opening 
  
parameters {
  choice choices: ['development', 'master'], description: 'please enter the branch name', name: 'BranchName'
}

tools {

maven "maven3.9.0"

}

agent any 

stages { //stages opening 

stage('checkout the code') {

steps {

sendSlackNotifications('STARTED')

git credentialsId: '2b3533b2-2eb0-4b48-98c5-87b7ad40c9b7', url: 'https://github.com/JashuwaJioTechnologies/maven-web-application.git'

}

}
stage('Build') {

steps {

sh "mvn clean package"

}

}
stage('Execute the SonarQube Report') {

steps {

sh "mvn clean sonar:sonar"

}

}
stage('Upload Build Artifact into nexus repository') {

steps {

sh "mvn clean deploy"

}


}
stage('Deploy the application') {

steps {

sshagent(['a053b86b-8d97-4538-b99d-2da56f92b135']) {

sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.127.43.31:/opt/apache-tomcat-9.0.73/webapps"

}

}

}

} //stages closing 

post {
  success {
    sendSlackNotifications(currentBuild.result)
  }
  failure {
    sendSlackNotifications(currentBuild.result) 
  }
}


} //pipeline closing 

//SlackNotifications 

def sendSlackNotifications(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    colorName = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESS') {
    colorName = 'GREEN'
    colorCode = '#00FF00'
  } else {
    colorName = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary, channel: '#tmobile-prod')
}
