pipeline {
	agent any
	
	environment{
	//insert the respective tag number for ndk repo to get the required code,ndk - 'git-vizio-ndk-bsp' - "$VERSION_RELEASE" / 'git-ndk' - "$VERSION_RELEASE"
	VERSION_RELEASE='4.0.434.0'
	}
	
  	stages {
		stage('Clone ndk & ndk_bsp tags') {
			 steps {
				 script {
						
						withCredentials([sshUserPrivateKey(credentialsId: 'git-vizio-ndk-bsp', keyFileVariable: '', passphraseVariable: '', usernameVariable: '')]) 
						{
                                       			 sh '''
								
								rm -rf *
								git clone --branch ${VERSION_RELEASE} git@github.com:BuddyTV/vizio_ndk_bsp.git
								cd vizio_ndk_bsp/ndk
								git submodule init
								git submodule update
								
							'''
                                       		}
				 }
				 
			 }
		 }
		
		stage("pack the files") {
			steps{
				//creating a tar file for the requested tag number
				sh ''' 
				cd vizio_ndk_bsp
				rm -rf  .git  .gitattributes  .gitignore  .gitmodules
				cd ..
				cd vizio_ndk_bsp/ndk/public
				rm -rf .git .gitignore
				cd ../../..
				tar -czvf $VERSION_RELEASE-ndk_bsp.tar.gz vizio_ndk_bsp
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
                               		   	"pattern": "(*)-ndk_bsp.tar.gz",
                               		  	"target": "vizio-dallas-megha-test/${VERSION_RELEASE}/"    
                              			}
                           			]
                       		   }"""
              			)
           		}
		}
		
	}
}
