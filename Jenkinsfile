@Library('jenkins_shared_lib_java_app') _
pipeline{

    agent any
    tools {
        maven 'maven-latest'
        jdk 'jdk17'
    }

    parameters {
    choice(name: 'action', choices: ['create', 'delete'], description: 'Choose action')
    string(name: 'Project', description: "name of the docker build", defaultValue: 'jenkins_shared_lib_java_ap')
    string(name: 'ImageTag', description: "tag of the docker build", defaultValue: 'v1')
    string(name: 'HubUser', description: "app name of the docker build", defaultValue: 'nrjydv1997')
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

        stage('MVN Build'){
            when { expression { params.action == 'create' } }
            steps{
                script{
                    mvnBuild()
                }
            }
        }

        stage('Docker Build'){
            when { expression { params.action == 'create' } }
            steps{
                script{
                    dockerBuild("${params.Project}", "${params.ImageTag}", "${params.HubUser}")
                }
            }
        }

        stage('Docker image scan'){
            when { expression { params.action == 'create' } }
            steps{
                script{
                    dockerImageScan("${params.Project}", "${params.ImageTag}", "${params.HubUser}")
                }
            }
        }
         
        stage("Docker Image Push"){
            when { expression { params.action == 'create' } }
            step{
                script{
                    dockerImagePush("${params.Project}", "${params.ImageTag}", "${params.HubUser}")
                }
            }
        }
    }
}