pipeline {
  agent any
  environment {
       PATH = "/opt/maven/bin:$PATH"
  }

  stages {
       stage("build") {
          steps {
              sh 'mvn clean deploy'
          }
        }
        stage('SonarQube analysis')
           environment {
               scannerHome = tool 'test-sonar-scanner'	   
           }
           steps {
               withSonarQubeEnv('test-sonarcube-server') {  
                    sh "${scannerHome}/bin/sonar-scanner"

                }
            }
         }
     }
}

