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
        eventTrigger jmespathQuery("repository.full_name=='dylanmeh/Using_Webhook2'")
    }


  stages {
    stage('Run maven') {
      steps {
        container('maven') {
          sh 'mvn -version'
          }
         }
       }  
     }
}