node {
echo "jobName is: ${env.JOB_NAME}"
echo "The Node Name is: ${env.NODE_NAME}"

def mavenHome = tool name: 'maven_3.8.4'

// get the code from github repo.
stage('CheckoutCode'){
git branch: 'development', credentialsId: 'a72f85fe-d16d-4580-96fb-d999180d9a60', url: 'https://github.com/Muzzammil476/maven-web-application.git'
    
}
// do the build using maven build tool
stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}
stage('ExecuteSonarQubeReport'){
sh "${mavenHome}/bin/mvn sonar:sonar"
}
stage('UploadArtifactsintoNexus'){
sh "${mavenHome}/bin/mvn deploy"
}
stage('DeployApplicationintoTomcatServer'){
sshagent(['1d8e807e-2116-4b55-8456-103a0ce36808']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@65.2.191.169:/opt/apache-tomcat-9.0.70/webapps/"
}
}
}
