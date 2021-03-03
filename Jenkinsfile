pipeline {
	agent any
	
	environment{
	GIT_TAG = "4.0.432.0-(SX7A_11.0.5.1_SX7B_7.0.5.1)"
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
		
		stage("download tagged code to workspace"){
				steps {
					sh '''
					git checkout ${GIT_TAG}
					sudo rm -rf ndk
					cd ..
					tar -czvf tag.tar.gz ndk_plus_bsp
					mv tag.tar.gz ndk_plus_bsp
					cd ndk_plus_bsp
					mv tag.tar.gz ${GIT_TAG}.tar.gz
					
					
					'''
					//mv tag.tar.gz ndk_plus_bsp
					//mv tag.tar.gz ${GIT_TAG}.tar.gz
					//mv !(ndk) tag
					//cd ..
					//tar -czvf ${GIT_TAG}.tar.gz tag
	//rm -rf Tag_${GIT_TAG} Tag_${GIT_TAG}@tmp
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
                               		   "pattern": "${GIT_TAG}.tar.gz",
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
