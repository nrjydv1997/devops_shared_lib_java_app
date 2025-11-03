@Library('jenkins_shared_lib_java_app') _
pipeline{

    agent any
    tools {
        maven 'maven-latest'
        jdk 'jdk17'
    }

    parameters{

        choice(name: 'action',choices: 'create\ndelete',description: 'Choose create/delete')
}

    
    stages{
        stage('Clean Workspace') {
            when{ expression { param.action =='create' } }
            steps {
                echo "🧹 Cleaning workspace before checkout..."
                cleanWs()  // Jenkins built-in step from Workspace Cleanup Plugin
            }
        }
        
        stage("Git Checkout"){
            when{ expression { param.action =='create' } }
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
            when{ expression { param.action =='create' } }
            steps{
                script{
                    mvnTest()
                }
            }
        }

        stage('MVN IntegrationTest'){
            when{ expression { param.action =='create' } }
            steps{
                script{
                    mvnIntegrationTest()
                }
            }
        }

        stage('Sonar StaticCodeAnalysis'){
            when{ expression { param.action =='create' } }
            steps{
                script{
                    staticCodeAnalysis()
                }
            }
        }
    }
}