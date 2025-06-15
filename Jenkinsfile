pipeline{
        agent any
            tools{
                maven 'maven'
            }
        stages{
            // sonarcloud analysis
            stage('CompileandRunSonarAnalysis'){
                steps{
                    withCredentials([string(credentialsId: 'samueltoken', variable: 'samueltoken')]) {
                        sh 'mvn clean verify sonar:sonar -Dsonar.login=$samueltoken -Dsonar.organization=samuel4 -Dsonar.host.url=https://sonarcloud.io -Dsonar.projectKey=samuel4'
                        
                    }
                }
            }
        }
    }