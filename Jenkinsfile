node {
   stage('SCM') {
	  git branch: 'main', url: 'https://github.com/samalasaikrishna/spring-petclinic.git'
   }
   stage ('build the packages') {
	  sh 'mvn install'
          }
   stage ('Junit tests') {
     //performs junit test cases
        junit 'target/surefire-reports/*.xml'
        }
   stage ('archiving') {
     //archiving the artifacts
     archiveArtifacts 'target/*.jar'
     }
     stage ('sonar') {
    // performing sonarqube analysis with "withSonarQubeENV(<Name of Server configured in Jenkins>)"
    withSonarQubeEnv('SonarQube_scan') {
      // requires SonarQube Scanner for Maven 3.2+
      sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar'
    }
  }

stage ('creating_zip') {
//this will create a zip file out of currently builded jar/ear/war file and stores zip in workspace
fileOperations([fileZipOperation(folderPath: 'target/spring-petclinic-2.4.5.jar', outputFolderPath: 'zip_test')])
//fileOperations([fileRenameOperation(destination: 'zip_test/${BUILD_NUMBER}-spring-petclinic.zip', source: 'zip_test/*.zip')])
}
        stage ('renaming_zip') {
//this will rename the zip file appending the build number
               // sh 'cd ${WORKSPACE}/zip_test'
                sh 'mv ${WORKSPACE}/zip_test/*.zip ${WORKSPACE}/zip_test/${BUILD_NUMBER}-spring-petclinic.zip'
}
stage ('Publish_Artifacts') {
//This will upload generated zip file to artifactory repo-spring-petclinic
     rtUpload (
             buildName: JOB_NAME,
                buildNumber: BUILD_NUMBER,
          serverId: 'JFrog',
           spec: '''{
              "files": [
                 {
                    "pattern": "zip_test/*.zip",
                    "target": "SDP-maven-Local",
                    "recursive": "false"
                 }
                        ]
                     }''')
    }
stage ('Publish build info') {
//This will publish the build info in json format to artifactory
            //steps {
                rtPublishBuildInfo (
                    buildName: JOB_NAME,
                    buildNumber: BUILD_NUMBER,
                    serverId: 'JFrog'
                                   )
                  //}
                             }
    stage('clean workspace') {
       sh 'mvn clean'
            sh 'rm -rf ${WORKSPACE}/zip_test/*'
        }

}
