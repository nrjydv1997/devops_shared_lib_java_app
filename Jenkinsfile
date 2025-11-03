@Library('jenkins_shared_lib_java_app') _
pipeline{

    agent any
    tools {
        maven 'maven-latest'
        jdk 'jdk17'
    }

    parameters {
    choice(name: 'action', choices: ['create', 'delete'], description: 'Choose action')
}


    
    stages{
        stage('Clean Workspace') {
            when { expression { params.action == 'create' } }
            steps {
                echo "🧹 Cleaning workspace before checkout..."
                cleanWs()  // Jenkins built-in step from Workspace Cleanup Plugin
            }
        }
        
        stage("Git Checkout"){
            when { expression { params.action == 'create' } }
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
            when { expression { params.action == 'create' } }
            steps{
                script{
                    mvnTest()
                }
            }
        }

        stage('MVN IntegrationTest'){
            when { expression { params.action == 'create' } }
            steps{
                script{
                    mvnIntegrationTest()
                }
            }
        }

        stage('Sonar StaticCodeAnalysis'){
            when { expression { params.action == 'create' } }
            steps{
                script{
                    def SonarQubeCredentialsId = 'sonar-api'
                    staticCodeAnalysis(SonarQubeCredentialsId)
                }
            }
        }

        stage('Quality Gate Status check'){
            when { expression { params.action == 'create' } }
            steps{
                script{
                    def SonarQubeCredentialsId = 'sonar-api'
                    qualityGateStatus(SonarQubeCredentialsId)
                }
            }
        }
    }
}