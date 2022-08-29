pipeline { 
    agent any 
    stages {
        stage('Build') { 
            steps {
                withMaven(maven : 'Apache_Maven 3.6.3'){
                        sh "mvn clean compile"
                }
            }
        }
        stage('Test'){
            steps {
                withMaven(maven : 'Apache_Maven 3.6.3'){
                        sh "mvn test"
                }

            }
        }
        stage('build && SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonar.tools.devops.****') {
                    sh 'sonar-scanner -Dsonar.projectKey=myProject -Dsonar.sources=./src'
                }
            }
        }
        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    // Requires SonarScanner for Jenkins 2.7+
                    waitForQualityGate abortPipeline: true
                }
            }
			}
        stage('Deploy') {
            steps {
               withMaven(maven : 'Apache_Maven 3.6.3'){
                        sh "mvn deploy"
                }

            }
        }
    }
}
