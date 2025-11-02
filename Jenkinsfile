@Library('jenkins_shared_lib_java_app') _
pipeline{

    agent any
    tools {
        maven 'maven-latest'
        jdk 'jdk17'
    }
    stages{
        stage("Git Checkout"){
            steps{
                script{
                    gitCheckout(
                        branch: "main" ,
                        url: "https://github.com/nrjydv1997/devops_shared_lib_java_app.git"
                         
                        )
                }
            }
        }

        stage('MVN Test'){
            steps{
                script{
                    mvnTest()
                }
            }
        }
    }
}