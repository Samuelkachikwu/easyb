pipeline{
        agent any
            tools{
                maven 'maven'
            }
        stages{
            // sonarcloud analysis
            stage('CompilingandRunSonarAnalysis'){
                steps{
                    withCredentials([string(credentialsId: 'sammytoken', variable: 'sammytoken')]) {
                        sh 'mvn clean verify sonar:sonar -Dsonar.login=$sammytoken -Dsonar.organization=sammy -Dsonar.host.url=https://sonarcloud.io -Dsonar.projectKey=sammy'
                        
                    }
                }
            }
        }
    }