pipeline{
    agent{label "Jenkis-Agent"}
    
    environment {
        APP_NAME ="register-app-pipeline"
    }

    stages {
        stage ('cleanup workspace'){
            steps {
                cleanWs()
            }
        }

        stage ("checkout form SCM") {
            steps{
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/ganesh-devops/project-01-gitops-register-app-CD.git'
            }
        }

        stage ("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage ("push changed deployment file to GIT") {
            steps{
                sh """
                   git config --global user.name "ganesh-devops"
                   git config --global user.email "ganesh.bandari@outlook.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                  sh "git push https://github.com/ganesh-devops/project-01-gitops-register-app-CD.git main"
                }
            }
        }
    }


}