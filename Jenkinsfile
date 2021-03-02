pipeline {
	agent any
	environment{
	git_tag_name = "4.0.431.0-(SX7A_11.0.5.1_SX7B_7.0.5.1)"
	
	
	
	}
  	stages {
		stage('Clone ndk repo') {
			 steps {
               			 script {
					//checkout ndk and bsp
					dir("ndk_tags_code") {
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
		
		stage('Uploading ndk & bsp to artifactory') {
            	steps {
                	rtUpload (
                   		serverId: 'artifactory',
                   		spec:
                       		  """{
                           		"files": [
                              		 {
                               		   "pattern": "nd-bsp",
                               		   "target": "vizio-dallas-megha-test/"           								  
                              		 }
                           		 ]
                       		  }"""
              		)
           	}
		}
		
		
		 		
	}
}
