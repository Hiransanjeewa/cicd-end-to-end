pipeline {
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
        DOCKER_HUB_CREDENTIALS = 'Github-CI-CD'
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'Github-CI-CD', 
                url: 'https://github.com/Hiransanjeewa/cicd-end-to-end',
                branch: 'main'
            }
        }

        stage('Build Docker') {
            steps {
                script {
                    echo 'Build Docker Image'
                    def customImage = docker.build("hiransanjeewa/django:${BUILD_NUMBER}", '.')
                }
            }
        }

        stage('Push the artifacts') {
            steps {
                script {
                    echo 'Push to Repo'
                    docker.withRegistry('https://hub.docker.com', DOCKER_HUB_CREDENTIALS) {
                        customImage.push()
                    }
                }
            }
        }

        stage('Checkout K8S manifest SCM') {
            steps {
                git credentialsId: 'Github-CI-CD', 
                url: 'https://github.com/Hiransanjeewa/cicd-end-to-end.git',
                branch: 'main'
            }
        }

        stage('Update K8S manifest & push to Repo') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'Github-CI-CD', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        sh '''
                        cat deploy.yaml
                        sed -i '' "s/32/${BUILD_NUMBER}/g" deploy.yaml
                        cat deploy.yaml
                        git add deploy.yaml
                        git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
                        git remote -v
                        git push https://github.com/Hiransanjeewa/cicd-end-to-end.git HEAD:main
                        '''                        
                    }
                }
            }
        }
    }
}
















// pipeline {
    
//     agent any 
    
//     environment {
//         IMAGE_TAG = "${BUILD_NUMBER}"
//          DOCKER_BINARY_PATH = "/usr/local/bin/" 
//     }
    
//     stages {
        
//         stage('Checkout'){
//            steps {
//                 git credentialsId: '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',
//                 url: 'https://github.com/Hiransanjeewa/cicd-end-to-end',
//                 branch: 'main'
//            }
//         }


//         stage('Build Docker'){
//             steps{
//                 script{
//                     sh '''
//                     echo 'Buid Docker Image'
//                     PATH=\$DOCKER_BINARY_PATH:\$PATH docker build -t hiransanjeewa/django:7 .
//                                   '''
//                 }
//             }
//         }

//         stage('Push the artifacts'){
//            steps{
//                 script{
//                     sh '''
//                     echo 'Push to Repo'
//                     docker push hiransanjeewa/django:${BUILD_NUMBER}
//                     '''
//                 }
//             }
//         }
        
//         stage('Checkout K8S manifest SCM'){
//             steps {
//                 git credentialsId: "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",
//                 url: 'https://github.com/Hiransanjeewa/Audiohub-Kubernetes-manifest.git',
//                 branch: 'main'
//             }
//         }
        
//         stage('Update K8S manifest & push to Repo'){
//             steps {
//                 script{
//                     withCredentials([usernamePassword(credentialsId:  "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", passwordVariable: ' ', usernameVariable: ' ')]) {
//                         sh '''
//                         cat deploy.yaml
//                         sed -i '' "s/32/${BUILD_NUMBER}/g" deploy.yaml
//                         cat deploy.yaml
//                         git add deploy.yaml
//                         git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
//                         git remote -v
//                         git push https://github.com/Hiransanjeewa/Audiohub-Kubernetes-manifest.git HEAD:main
//                         '''                        
//                     }
//                 }
//             }
//         }
//     }
// }
