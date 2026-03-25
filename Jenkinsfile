pipeline {
  agent any
  environment {
       PATH = "/opt/maven/bin:$PATH"
  }

  stages {
       stage("build") {
          steps {
              echo "---------- Build Started ----------"

              sh 'mvn clean deploy -Dmaven.test.skip=true'

              echo "---------- Build Completed ----------"
          }
        }

        stage("test") {
          steps {
              echo "---------- Unit Test Started ----------"

              sh 'mvn surefire-report:report'

              echo "---------- Unit Test Completed ----------"
          }
        }

        stage('SonarQube analysis') {
           environment {
               scannerHome = tool 'test-sonar-scanner'	   
           }
           steps {
               withSonarQubeEnv('test-sonarcube-server') {  
                    sh "${scannerHome}/bin/sonar-scanner"

                }
            }
         }
        stage("QualityGate") {
            steps {
                script {
                    timeout(time: 1, unit: 'HOURS') {
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                          error "Pipeline aborted due to quality gate failure: ${qg.status}"
                       }
                    }
                }
              }
         }
    }
}

