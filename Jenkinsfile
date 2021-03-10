pipeline {
	agent any
	
	environment{
	//insert the respective tag number for ndk repo to get the required code,ndk - 'git-vizio-ndk-bsp' - "$VERSION_RELEASE" / 'git-ndk' - "$VERSION_RELEASE"
	tag='4.0.434.0'
	}
	
  	stages {
		stage('Clone ndk & ndk_bsp tags') {
			 steps {
				 script {
						withCredentials([sshUserPrivateKey(credentialsId: 'git-ndk', keyFileVariable: '', passphraseVariable: '', usernameVariable: '')]) 
						{
                    					sh '''
								set -x
								rm -rf *
								mkdir ndk_tag
								cd ndk_tag
								git clone --branch ${tag} git@github.com:BuddyTV/ndk.git
							'''
						}
						withCredentials([sshUserPrivateKey(credentialsId: 'git-vizio-ndk-bsp', keyFileVariable: '', passphraseVariable: '', usernameVariable: '')]) 
						{
                                       			 sh '''
								set -x
								mkdir ndk_bsp_tag
								cd ndk_bsp_tag
							'''
							//	git clone --branch ${tag} git@github.com:BuddyTV/vizio_ndk_bsp.git
                                       		}
				 }
				 
			 }
		 }
		
		stage("pack the files") {
			steps{
				//creating a tar file for the requested tag number
				sh ''' 
				cd ndk_tag
				tar -czvf $tag-ndk.tar.gz ndk
				cd ..
				
				
				'''
				//cd ndk_bsp_tag
				//tar -czvf $tag-ndk_bsp.tar.gz vizio_ndk_bsp
				//cd ..
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
                               		   	"pattern": "ndk_tag/(*)-ndk.tar.gz",
                               		  	 "target": "vizio-dallas-megha-test/${tag}/"    
                              			},
						{
                               		   	"pattern": "ndk_bsp_tag/(*)-ndk_bsp.tar.gz",
                               		  	 "target": "vizio-dallas-megha-test/${tag}/"    
                              			}
						
                           			]
                       		  }"""
              			)
           		}
		}
		
	}
}
