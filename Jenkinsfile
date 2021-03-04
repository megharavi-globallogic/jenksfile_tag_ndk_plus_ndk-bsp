pipeline {
	agent any
	
	environment{
	GIT_TAG ='4.0.436.0-(SX7A_11.0.5.1_SX7B_7.0.5.1)'
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
								git clone --branch ${GIT_TAG} git@github.com:BuddyTV/ndk.git
							'''
						}
				 }
			 }
		 }
		
		stage("pack the files") {
			steps{
				sh ''' 
				set -x
				echo ${GIT_TAG}
				'''
			}
							//tar -czvf $GIT_TAG.tar.gz ndk

		}
					//git checkout ${GIT_TAG}

		stage('Uploading ndk & bsp to artifactory') {
            		steps {
                		rtUpload (
                   		serverId: 'artifactory',
                   		spec:
                       	 	    """{
                           		"files": [
                              	 		{
                               		   	"pattern": "${GIT_TAG}.tar.gz",
                               		  	 "target": "vizio-dallas-megha-test/ndk"           								  
                              			}
                           			]
                       		  }"""
              			)
			//sh 'rm -rf *.tar.gz'
           		}
		}
		
	}
}
