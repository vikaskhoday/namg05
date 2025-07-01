pipeline {
    agent {label 'maven'}

environment {
    PATH= "/opt/apache-maven-3.9.10/bin:$PATH"
}
    stages {
       stage("build"){
        steps {
           sh 'mvn clean deploy -Dmaven.test.skip=true'
        }
      }
    }
    stage('test'){
        steps{
            echo "==============unit test started====="
            sh 'mvn surefire-report:report'
            echo "==============unit test completed==="
        }
    }
    stage('SonarQube analysis'){
    environment {
        scammerHome = tool 'namg-snar-scanner' 
    }
    steps {
    withSonarQubeEnv('namg-sonarqube-server') {
        sh "${scannerHome}/bin/sonar-scanner"
        }
     }  
    }
    stage{"Quality Gate"} {
        steps {
            script {
                timeout(time:1,unit: 'HOURS') {
                    def qg = waitForQualityGate()
                    if (qg.status != 'OK') {
                        error " Piepline aborted due to quality gate failure: ${qg.status}"
                    }  
                  }

                }
            }
        }
    }   


    



