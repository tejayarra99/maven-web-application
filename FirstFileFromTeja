node{

def mavenHome = tool name: "maven 3.9.3"

stage('CheckOutCode'){
	git branch: 'development', credentialsId: '43b5eda0-adca-49cf-b03c-65d8f4fe9d80', url: 'https://github.com/tejayarra99/maven-web-application.git'
}


stage('Build'){
sh "${mavenHome}/bin/mvn clean package"

}

stage('SonarQubeReport'){

sh "${mavenHome}/bin/mvn clean sonar:sonar"

}

stage ('uploadArtifactsIntoNexus'){

sh "${mavenHome}/bin/mvn clean deploy"

}

stage ('DeployApplicationIntoTomcat'){

sshagent(['59472bce-9ad6-4c59-b939-48e662a4cfae']) {
 sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@35.154.34.177:/opt/apache-tomcat-9.0.79/webapps/"
}

}

stage('sendEmailNotification'){

emailext body: 'build completed..!', subject: 'build complete', to: 'tejayarra99@gmail.com'

}

}
