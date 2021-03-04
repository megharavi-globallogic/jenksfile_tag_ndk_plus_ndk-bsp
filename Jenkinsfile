pipeline {
	agent any
	
	environment{
	TAG ='4.0.436.0'
	ndk_bsp_tag = 'cbbccnk'
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
								git clone --branch ${TAG} git@github.com:BuddyTV/ndk.git
							'''
						}
				 }
			 }
		 }
		
		stage("pack the files") {
			steps{
				sh ''' 
				set -x
				tar -czvf $TAG.tar.gz ndk
				'''
			}
							

		}
		

		stage('Uploading ndk & bsp artifactory') {
            		steps {
                		rtUpload (
                   		serverId: 'artifactory',
                   		spec:
                       	 	    """{
                           		"files": [
                              	 		{
                               		   	"pattern": "(*).tar.gz(*)",
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
