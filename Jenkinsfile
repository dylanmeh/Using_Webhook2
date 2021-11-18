pipeline {
  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        spec:
          containers:
          - name: maven
            image: maven:alpine
            command:
            - cat
            tty: true
        '''
    }
  }

  triggers {
        eventTrigger jmespathQuery("repository.url=='https://github.com/dylanmeh/Using_Webhook2'")
    }


  stages {
    stage('First stage') {
        	steps {
            	script {
                	echo "Build triggered by:" + currentBuild.getBuildCauses()[0].toString()
                	def cause = currentBuild.getBuildCauses()[0];
               // We check that the job is triggered by event, to avoid NullPointerException when using the json content.
                	if ( cause._class.contains("EventTriggerCause") ) {
                    	echo "Job triggered by event"
                    	//The json payload is enclosed in an event field
                  	  def eventContent = cause.event
                    	echo eventContent.fact.toString()
                    	//You can now use any json field send by the cURL command
                    	def fact = eventContent.fact.content
                    	echo "Fact is:" + fact.toString()
                	}
                	else if ( cause._class.contains("UserIdCause") ) {
                    	// The job can still be run manually, you may want to have different behavior.
                    	echo "Job triggered by user"
                	}
                	else {
                    	echo "Job triggered by something else"
                	}
              }
          }
    stage('Run maven') {
      steps {
        container('maven') {
          sh 'mvn -version'
          }
         }
       }  
     }
   }
 }
}
