pipeline {
    agent {label 'maven'}

environment {
    PATH= "/opt/apache-maven-3.8.9/bin:$PATH"
}
    stages {
       stage("build"){
        steps {
           sh 'mvn clean deploy'
        }
      }
    }
}
