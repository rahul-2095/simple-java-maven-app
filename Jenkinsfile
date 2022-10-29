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
                    withSonarQubeEnv(credentialsId: 'sonarqube_secret') {
                        sh "${tool("sonarqube_4.7.0.2747")}/bin/sonar-scanner \
                        -Dsonar.projectKey=simple_java_maven_app \
                        -Dsonar.sources=. \
                        -Dsonar.java.binaries=target \
                        -Dsonar.host.url=http://172.31.21.19:9000 \
                        -Dsonar.login=c5c0fb5b4e4b222b4a84b9004d2f10b15df9d227"
   
                    }
                }
            }

        }
        stage("upload artifact"){
            steps{
               sh 'mvn -s settings.xml deploy'
            }
        }
       
       

    }

}

