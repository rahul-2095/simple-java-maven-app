pipeline{
    agent any
    tools{
        maven 'maven_3.8.6'
    }
    stages{
        stage("build"){
            steps{
                script{
                    sh 'mvn install'
                }

            }
        }
        stage("unit testing"){
            steps{
               
                sh 'mvn test'
            }
            post{
                success{
                     echo "junit testing is success,publishing report"
                     junit 'target/surefire-reports/*.xml'
               
                }
                failure{
                    echo "junit testing is failed"

                }
            }
        }
        stage("sonar"){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'mysonar') {
                        sh "${tool("sonarqube_4.7.0.2747")}/bin/sonar-scanner \
                        -Dsonar.projectKey=simple_java_maven_app \
                        -Dsonar.sources=. \
                        -Dsonar.java.binaries=target \
                        -Dsonar.host.url=http://172.31.21.19:9000 \
                        -Dsonar.login=sqp_180be31a1cea1d1d02bb837ededc86f2d942edfc"
   
                    }
                }
            }

        }
        stage("upload artifact"){
            steps{
               sh 'mvn deploy'
            }
        }
       
       

    }

}

