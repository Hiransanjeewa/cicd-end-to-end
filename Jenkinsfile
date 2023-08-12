pipeline {
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
        DOCKER_HUB_CREDENTIALS = 'Dockerhub-Credentials'
     GIT_USERNAME = 'GIT_USERNAME'
     //   GIT_TOKEN = 'Github-Token'
        GIT_PASSWORD= 'GIT_PASSWORD'
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'Github-Credentials', 
                url: 'https://github.com/Hiransanjeewa/cicd-end-to-end',
                branch: 'main'
            }
        }

        stage('Build Docker') {
            steps {
                script {
                    echo 'Build Docker Image'
                    customImage = docker.build("hiransanjeewa/django:${BUILD_NUMBER}", '.')
                }
            }
        }

        stage('Push the artifacts') {
            steps {
                script {
                    echo 'Push to Repo'
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_HUB_CREDENTIALS) {
                        customImage.push()
                    }
                }
            }
        }

        stage('Checkout K8S manifest SCM') {
            steps {
                git credentialsId: 'Github-Credentials', 
                url: 'https://github.com/Hiransanjeewa/Audiohub-Kubernetes-manifest',
                branch: 'main'
            }
        }

        stage('Update K8S manifest & push to Repo') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'Github-Token', variable: 'GITHUB_TOKEN')]) {
                        sh '''
                        cat deploy.yaml
                        ls
                        pwd
                        chmod +rwx deploy.yaml
                        sed -i -e "s/django:1/django:2/g" deploy.yaml
                                  
                        cat deploy.yaml
                        git status
                        git add deploy.yaml
                        git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
                        git remote -v
                        git push https://${GITHUB_TOKEN}@github.com/hiransanjeewa/Audiohub-Kubernetes-manifest.git HEAD:main
                        



                        '''                        
                    }
                }
            }
        }
    }
}


                      //  git push origin main

                   //     git remote set-url origin https://${GIT_PASSWORD}@github.com/${GIT_USERNAME}/Audiohub-Kubernetes-manifest.git

                 //        git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/hiransanjeewa/Audiohub-Kubernetes-manifest HEAD:main


  //  lineNumber = 19
                    //  newLine = "image: hiransanjeewa/django:${BUILD_NUMBER}"
                    
                    //  lines = readFile(filePath).split("\n")
                    // lines.eachWithIndex { line, index ->
                    //     if (index == lineNumber - 1) {
                    //         lines[index] = newLine
                    //     }
                    // }
                    
                    // writeFile file: filePath, text: lines.join('\n')









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
