pipeline {
    
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
        
        stage('Checkout'){
           steps {
                git credentialsId: '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',
                url: 'https://github.com/Hiransanjeewa/cicd-end-to-end',
                branch: 'main'
           }
        }

        stage('Build Docker'){
            steps{
                script{
                    sh '''
                    echo 'Buid Docker Image'
                    docker build -t hiransanjeewa/django:${BUILD_NUMBER} .
                    '''
                }
            }
        }

        stage('Push the artifacts'){
           steps{
                script{
                    sh '''
                    echo 'Push to Repo'
                    docker push hiransanjeewa/django:${BUILD_NUMBER}
                    '''
                }
            }
        }
        
        stage('Checkout K8S manifest SCM'){
            steps {
                git credentialsId: "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",
                url: 'https://github.com/Hiransanjeewa/Audiohub-Kubernetes-manifest.git',
                branch: 'main'
            }
        }
        
        stage('Update K8S manifest & push to Repo'){
            steps {
                script{
                    withCredentials([usernamePassword(credentialsId:  "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", passwordVariable: ' ', usernameVariable: ' ')]) {
                        sh '''
                        cat deploy.yaml
                        sed -i '' "s/32/${BUILD_NUMBER}/g" deploy.yaml
                        cat deploy.yaml
                        git add deploy.yaml
                        git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
                        git remote -v
                        git push https://github.com/Hiransanjeewa/Audiohub-Kubernetes-manifest.git HEAD:main
                        '''                        
                    }
                }
            }
        }
    }
}
