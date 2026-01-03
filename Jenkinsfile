pipeline {
    // These are pre-build sections
    agent {
        node {
            label 'AGENT-1'
        }
    }
    environment {
        COURSE = "Jenkins"
        appVersion = ""
        ACC_ID = "930832106480"
        PROJECT = "roboshop"
        COMPONENT = "catalogue"
        REGION = "us-east-1"
    }
    options {
        timeout(time: 30, unit: 'MINUTES') 
        disableConcurrentBuilds()
    }
    parameters {
        string(name: 'appVersion', description: 'Which app version you want to deploy')
        choice(name: 'deploy_to', choices: ['dev', 'qa', 'produ'], description: 'Pick something')
        
    }
    // This is build section
    stages {
        
        stage('Deploy') {
            steps {
                script{
                    withAWS(region:'us-east-1',credentials:'aws-creds') {
                        sh """
                            aws eks update-kubeconfig --region ${REGION} --name ${PROJECT}-${params.deploy_to}
                        """
                    }
                }
            }
        }
        
    }
        
    


    post {
        always {
            echo 'Pipeline finished'
            cleanWs()
        }
        success {
            echo 'Pipeline succeeded'
        }
        failure {
            echo 'Pipeline failed'
        }
        aborted {
            echo 'Pipeline aborted'
        }
    }
}