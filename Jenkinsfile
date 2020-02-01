pipeline {
    agent any 
    tools { 
        maven 'maven' 
      
    }
stages { 
     
 stage('Preparation') { 
     steps {
// for display purpose

      // Get some code from a GitHub repository

      git 'https://github.com/suresh352/game-of-life.git'

      // Get the Maven tool.
     
 // ** NOTE: This 'M3' Maven tool must be configured
 
     // **       in the global configuration.   
     }
   }

   stage('Build') {
       steps {
       // Run the maven build

      //if (isUnix()) {
         sh 'mvn -Dmaven.test.failure.ignore=true install'
      //} 
      //else {
      //   bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
       }
//}
   }
 
    stage('Unit Test Results') {
        steps {
        junit '**/target/surefire-reports/TEST-*.xml'
      
        }
   }
 stage('sonarqube1') {
    environment {
       scannerHome = tool 'sonarqube1'
    }
    steps {
      withSonarQubeEnv('sonarqube1') {
            sh "${scannerHome}/bin/sonar-scanner"
          }
          timeout(time: 12, unit: 'MINUTES') {
              waitForQualityGate abortPipeline: true
          }
      }
  }
     stage('Artifact upload') {
      steps {
       nexusPublisher nexusInstanceId: 'localnexus-3', nexusRepositoryId: 'nexus-spring', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'gameoflife-web/target/gameoflife.war']], mavenCoordinate: [artifactId: 'gameoflife', groupId: 'com.wakaleo.gameoflife', packaging: 'war', version: '$BUILD_NUMBER']]]      
      }
     }
    //stage('Deploy War') {
      //steps {
        //sh label: '', script: 'ansible-playbook deploy.yml'
      //}
 //}
}
//post {
    //    success {
      //      archiveArtifacts 'gameoflife-web/target/*.war'
        //}
       // failure {
         //   mail to:"sankar.dadi@qentelli.com", subject:"FAILURE: ${currentBuild.fullDisplayName}", body: "Build failed"
        //}
    //}       
}
