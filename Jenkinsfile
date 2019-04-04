node {
   //def mvnHome
   def gradleHome
  stage('Checkout stage') { // for display purposes
      // Get some code from a GitHub repository
      git branch: 'artifactIntegration',url: 'https://github.com/gaurav1987singh/javaProjectWithGradle.git'
      echo "In Checkout stage"
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      //mvnHome = tool name: 'maven354', type: 'maven'
      gradleHome = tool name: 'gradle45', type: 'gradle' 
   }
   
   stage ('Clean'){
     if (isUnix()) {
        echo "Inside Unix System"
        sh "'${gradleHome}/bin/gradle' -v"
         sh "'${gradleHome}/bin/gradle' clean"
      } else {
         echo "Inside Windows System"
         bat script: "${gradleHome}\\bin\\gradle clean"
      }  
       
   }
   stage('Build') {
      // Run the maven build
      if (isUnix()) {
         sh "'${gradleHome}/bin/gradle' build -x test"
      } else {
         bat script: "${gradleHome}\\bin\\gradle build -x test"
      }
   }
   stage('UnitTest') {
     if (isUnix()) {
       sh "'${gradleHome}/bin/gradle' test"
       } else {
         
       bat script: "${gradleHome}\\bin\\gradle test"
      }
   }
   
   stage('SonarQube Analysis') {
      echo "SonarQube Analysis begin"
      if (isUnix()) {
         echo "Running Sinarqube from  Unix System"
//sh "'${gradleHome}/bin/gradle' sonarqube -Dsonar.host.url=http://localhost:9000 -Dsonar.login=860b7e9e06174ee34335ad0f6f200f4fc8737725" 
      } else {
         echo "Running Sonarqube from  Windows System"
bat script: "${gradleHome}\\bin\\gradle sonarqube -Dsonar.host.url=http://localhost:9000 -Dsonar.login=860b7e9e06174ee34335ad0f6f200f4fc8737725"
      }
   }
      stage ('Deploy to Maven local'){
          echo "Deploy to Maven local begins"
         if (isUnix()) {
       sh "'${gradleHome}/bin/gradle' uploadArchives -i"
       } else {
          bat script: "${gradleHome}\\bin\\gradle uploadArchives -i"
         }  
      }
      
      stage('Deploy to Artifactory cloud'){
      def server = Artifactory.server 'manjuArtifactory'
      echo "$server"
      server.bypassProxy = true
      server.credentialsId = 'manjuArtifactory'
      server.connection.timeout = 300
      def uploadSpec = """{
 "files": [
  {
      "pattern": "${WORKSPACE}/build/libs/gradlePipeline_forJavaProject-1.0.jar",
      "target": "gauravLocalRepo/"
    }
 ]
   }"""
server.upload(uploadSpec)
  } 
  
  stage('Deploy to Artifactory using gradle'){
      //sh label: '', script: "curl -uadmin:APAtN1w4MuidS5RwTrPPwvSmQtr -T ${WORKSPACE}/build/libs/gradlePipelineDemo_forJavaProject-1.0.jar https://artifactoryg01dy.jfrog.io/artifactoryg01dyg01dy/libs-snapshot/com/sample/program/gradlePipelineDemo_forJavaProject/1.0"
     if (isUnix()) {
        echo "Running Artifactory from  Unix System"
       sh "'${gradleHome}/bin/gradle' artifactoryPublish"
     } else {    
     //bat script: "${gradleHome}\\bin\\gradle artifactoryPublish"   
  }
 }
}
