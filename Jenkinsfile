pipeline {
	agent any
	
	environment{
	GIT_TAG = "4.0.431.0-(SX7A_11.0.5.1_SX7B_7.0.5.1)"
	}
	
  	stages {
		stage('Clone ndk repo') {
			 steps {
               			 script {
					//checkout ndk and bsp
					dir("ndk_plus_bsp_code") {
						withCredentials([sshUserPrivateKey(credentialsId: 'git-ndk', keyFileVariable: '', passphraseVariable: '', usernameVariable: '')]) 
						{
                    					sh '''
								set -x
								rm -rf *
								git clone git@github.com:BuddyTV/ndk
							'''
						}
					}
				 }
			 }
		 }
		
		stage('create tar.gz files'){
			steps{
				sh'''
				set -x
				rm -rf*
				tar -czvf ndk_zip.tar.gz ndk_plus_bsp_code
				'''
			}
		}
		
		stage("download tagged code to workspace"){
				steps {
					checkout scm: [$class: 'GitSCM', userRemoteConfigs: [[url: 'git@github.com:BuddyTV/ndk', credentialsId: 'git-ndk' ]], branches: [[name: '${GIT_TAG}']]]
				}
		}
			
		stage('Uploading ndk & bsp to artifactory') {
            	steps {
                	rtUpload (
                   		serverId: 'artifactory',
                   		spec:
                       		  """{
                           		"files": [
                              		 {
                               		   "pattern": "${GIT_TAG}.zip",
                               		   "target": "vizio-dallas-megha-test/"           								  
                              		 }
                           		 ]
                       		  }"""
              		)
           	}
		}		
	}
}
