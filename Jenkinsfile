pipeline {
	agent any
	
	environment{
	//insert the respective tag number for ndk repo to get the required code
	tag='4.0.436.0'
	}
	
  	stages {
		stage('Clone ndk&ndk_bsp repo') {
			 steps {
				 script {
						withCredentials([sshUserPrivateKey(credentialsId: 'git-ndk', keyFileVariable: '', passphraseVariable: '', usernameVariable: '')]) 
						{
                    					sh '''
								set -x
								rm -rf *
								git clone --branch ${tag} git@github.com:BuddyTV/ndk.git
							'''
						}
						withCredentials([sshUserPrivateKey(credentialsId: 'git-vizio-ndk-bsp', keyFileVariable: '', passphraseVariable: '', usernameVariable: '')]) 
						{
                                       			 sh '''
								set -x
								git clone --branch ${tag} git@github.com:BuddyTV/vizio_ndk_bsp.git
							'''
                                       		}
				 }
				 
			 }
		 }
		
		stage("pack the files") {
			steps{
				//creating a tar file for the requested tag number
				sh ''' 
				set -x
				tar -czvf $tag.ndk.tar.gz ndk
				tar -czvf $tag.ndk_bsp.tar.gz vizio_ndk_bsp
				
				'''
			}
							

		}
		

		stage('Upload ndk&ndk_bsp tar to artifactory') {
            		steps {
				//uploading to the tar file to the artifactory
                		rtUpload (
                   		serverId: 'artifactory',
                   		spec:
                       	 	    """{
                           		"files": [
                              	 		{
                               		   	"pattern": "(*).ndk.tar.gz",
                               		  	 "target": "vizio-dallas-megha-test/${tag}/"    
                              			},
						{
                               		   	"pattern": "(*).ndk_bsp.tar.gz",
                               		  	 "target": "vizio-dallas-megha-test/${tag}/"    
                              			}
						
                           			]
                       		  }"""
              			)
           		}
		}
		
	}
}
