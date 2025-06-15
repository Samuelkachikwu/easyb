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

            // snyk code analysis
            stage('RunSnykCodeAnalysis'){
                steps{
                    withCredentials([string(credentialsId:'snyk_token', variable: 'snyk_token')]) {
                        sh 'mvn snyk:test -fn'
                    }
                }
            }
            // building docker image
            stage("Build Docker image"){
                steps{
                    withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                        script{
                            app =  docker.build("javaapp")
                            
                        }
                        
                    }
                }
            }

                // push to aws ecr after build ..
                stage('PushToECR'){
                    steps{
                        script{
                            docker.withRegistry("https://924338258393.dkr.ecr.us-east-1.amazonaws.com", "ecr:us-east-1:aws-credentials") {
                                app.push("latest")
                            }
                        }
                    }
                

                }

        // deploy to kubernetes cluster on aws
            stage("DeployToKubernetes"){
                steps{
                    withKubeConfig([credentialsId: "kubelogin"]) {
                        sh 'kubectl delete all --all -n devsecops'
                        sh 'kubectl apply -f deployment.yaml -n devsecops'
                    }
                }
            }

            }
    }
