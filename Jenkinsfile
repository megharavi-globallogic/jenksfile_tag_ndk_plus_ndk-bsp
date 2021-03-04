pipeline {
	agent any
	
	environment{
	git_tag ='4.0.436.0'
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
								git clone --branch ${git_tag} git@github.com:BuddyTV/ndk.git
							'''
						}
				 }
			 }
		 }
		
		stage("pack the files") {
			steps{
				sh ''' 
				set -x
				tar -czvf $git_tag.tar.gz ndk
				'''
			}
							

		}
		
		stage('Uploading ndk & bsp to artifactory') {
            		steps {	
				def uploadSpec = """{
 		 		"files": [
 		  		 {
				 "pattern": "(*).tgz(*)",
     				 "target": "vizio-dallas-megha-test/ndk"
     				 "recursive": "false"
  				   }
		   		]
				}"""
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
