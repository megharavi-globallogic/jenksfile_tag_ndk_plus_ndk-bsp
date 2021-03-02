pipeline {
	agent any
	environment{
	git_tag_name = "4.0.431.0-(SX7A_11.0.5.1_SX7B_7.0.5.1)"
	
	
	
	}
  	stages {
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
		
		
		 stage('Checkout ndk & bsp tags from git') {
			 steps {
               			 script {
					//checkout ndk and bsp
					dir("ndk_tags") {
						sh 'git checkout ${git_tag_name}'
					}
					
				 }
			 }
		 }		
	}
}
