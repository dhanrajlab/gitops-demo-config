pipeline {
    agent any
    environment {
        APP_NAME = "gitops-demo-app"
        }
    stages {
        stage('Cleanup Workspace'){
            steps {
                script {
                    cleanWs()
                }
            }
        }
        stage('Checkout SCM'){
            steps {
                git credentialsId: 'github', 
                url: 'https://github.com/dhanrajlab/gitops-demo-config.git',
                branch: 'master'
            }
        }
        stage('Updating Kubernetes deployment file'){
            steps {
                sh "cat deployment.yml"
                sh "sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yml"
                sh "cat deployment.yml"
            }
        }
        stage('Push the changed deployment file to Git'){
            steps {
                script{
                    sh """
                    git config --global user.name "dhanrajlab"
                    git config --global user.email "calsoftpersonallab@gmail.com"
                    git add deployment.yml
                    git commit -m 'Updated the deployment file' """
                    withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'pass', usernameVariable: 'user')]) {
                        sh "git push https://$user:$pass@github.com/dhanrajlab/gitops-demo-config.git master"
                    }
                }
            }
        }
    }
}
