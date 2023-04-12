pipeline { //pipeline opening 

tools {

maven "maven3.9.0"

}

agent any 

stages { //stages opening 

stage('Checkout the code') {

steps {

git credentialsId: '2b3533b2-2eb0-4b48-98c5-87b7ad40c9b7', url: 'https://github.com/JashuwaJioTechnologies/maven-web-application.git'

}

}
stage('build') {

steps {

sh "mvn clean package"

}


}

} //stages closing 

} //pipeline closing
