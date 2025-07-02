pipeline {
    agent { label 'maven' }

    environment {
        PATH = "/opt/apache-maven-3.9.10/bin:$PATH"
    }

    stages {
        stage("Build") {
            steps {
                sh 'mvn clean deploy -Dmaven.test.skip=true'
            }
        }

        stage("Test") {
            steps {
                echo "============== Unit test started ====="
                sh 'mvn surefire-report:report'
                echo "============== Unit test completed ===="
            }
        }

        stage("SonarQube analysis") {
            environment {
                scannerHome = tool 'namg-sonar-scanner'
            }
            steps {
                withSonarQubeEnv('namg-sonarqube-server') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }

        stage("Quality Gate") {
            steps {
                script {
                    timeout(time: 1, unit: 'HOURS') {
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "Pipeline aborted duesss to quality gate failure: ${qg.status}"
                        }
                    }
                }
            }
        }
    }
}
