pipeline{

    agent any

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
    }
}