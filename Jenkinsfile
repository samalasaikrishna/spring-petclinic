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
      // archiveArtifacts 'target/*.war'
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
//fileOperations([fileZipOperation(folderPath: 'target/spring-petclinic-2.4.5.war', outputFolderPath: 'zip_test')])
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
      // sh 'mvn clean'
            sh 'rm -rf ${WORKSPACE}/zip_test/*'
        }

        stage('ansible_server'){
              //  sshPublisher(publishers: [sshPublisherDesc(configName: 'Ansible control server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook -i /home/ansible/playbooks/hosts /home/ansible/playbooks/spring-petclinic.yml', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '//home/ansible', remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'Jenkinsfile')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
        sshPublisher(publishers: [sshPublisherDesc(configName: 'Ansible control server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook -i /home/ansible/playbooks/hosts /home/ansible/playbooks/spring-petclinic.yml', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '**')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
        }

}

     //  stage('checksum') {
               // for (def module:buildInfo.modules) {  
              // def coordinates = module.id.toString().split(':')
                // def groupId = coordinates[0]
                // def artifactId = coordinates[1]
                //  def version = coordinates[2]
                //for (def artifact:module.artifacts) {
			//if(!artifact.name.endsWith(".pom"))
			  // {
                                //printf(artifact.sha1)
	                   // }
		          //   else {
			   //      printf(artifact.sha1)
			    //         }
		                 // }
       // }
       // }
        
        
        


        //stage ('checksum') {
          //      VAR=module.artifact.md5.@name:{{"$match":"${BUILD_NUMBER}-spring-petclinic.zip"}}
            //   echo $VAR
       // }



        //stage('DEPLOY'){
               // sshPublisher(publishers: [sshPublisherDesc(configName: 'ACS', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook -i playbooks/hosts playbooks/spring-petclinic.yml --user ansible', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
       // }
       // stage ('ACS') {
                //script {
                        //sshagent(credentials : ['jenkins-pem']) {
                        //sh "echo pwd"
                       // sh 'ssh -t -t ansible@ec2-3-142-205-70.us-east-2.compute.amazonaws.com -o StrictHostKeyChecking=no'
                       // sh "echo pwd"
                        //sh 'sudo -i -u root'
                       // sh 'cd playbooks'
                        //sh 'echo pwd'
                        //sh 'ansible-playbook -i hosts spring-petclinic.yml --user ansible'
                      // }
                // }


       // stage ('CD') {
                
              //  sshPublisher(publishers: [sshPublisherDesc(configName: 'ACS', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook -i hosts spring-petclinic.yml --user ansible', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
                
                
       // }
        
