pipeline {
	agent any
	
	environment{
	GIT_TAG = "test"
	}
	
  	stages {
		stage('Clone ndk repo') {
			 steps {
				 script {
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
		stage('create tar.gz files'){
			steps{
				sh'''
				set -x
				rm -rf *.tar.gz
				tar -czvf ndk_zip.tar.gz ndk
				'''
			}
		}
		stage("download tagged code to workspace"){
				steps {
					sh '''
	
					mkdir  ${GIT_TAG}
	
					cd ${GIT_TAG}
					git checkout ${GIT_TAG}
					
					'''
						//tar -czvf ${GIT_TAG}.tar.gz tag_${GIT_TAG}
					//rm -rf tag_${GIT_TAG} tag_${GIT_TAG}@tmp
	
					//checkout scm: [$class: 'GitSCM', userRemoteConfigs: [[url: 'git@github.com:BuddyTV/ndk', credentialsId: 'git-ndk' ]], branches: [[name: '${GIT_TAG}']]]

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
                               		   "pattern": "ndk.tar.gz",
                               		   "target": "vizio-dallas-megha-test/"           								  
                              		}
                           		 ]
                       		  }"""
              		)
			//sh 'rm -rf *.tar.gz'
           	}
		}
	}
}
