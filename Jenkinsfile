pipeline {
  2     agent {label 'maven'}
  3 
  4 environment {
  5     PATH= "/opt/apache-maven-3.9.10/bin:$PATH"
  6 }
  7     stages {
  8        stage("build"){
  9         steps {
 10            sh 'mvn clean deploy -Dmaven.test.skip=true'
 11         }
 12       }
 13     }
 14     stage('test'){
 15         steps{
 16             echo "==============unit test started====="
 17             sh 'mvn surefire-report:report'
 18             echo "==============unit test completed==="
 19         }
          }
 20     }
 21     stage('SonarQube analysis'){
 22     environment {
 23         scammerHome = tool 'namg-snar-scanner'
 24     }
 25     steps {
 26     withSonarQubeEnv('namg-sonarqube-server') {
 27         sh "${scannerHome}/bin/sonar-scanner"
 28         }
 29      }
 30     }
 31     stage{"Quality Gate"} {
 32         steps {
 33             script {
 34                 timeout(time:1,unit: 'HOURS') {
 35                     def qg = waitForQualityGate()
 36                     if (qg.status != 'OK') {
 37                         error " Piepline aborted due to quality gate failure: ${qg.status}"
 38                     }
 39                   }
 40 
 41                 }
 42             }
 43         }
 44     }
 45 
