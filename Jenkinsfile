pipeline {
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
        DOCKER_HUB_CREDENTIALS = 'Dockerhub-Credentials'
     GIT_USERNAME = 'GIT_USERNAME'
     //   GIT_TOKEN = 'Github-Token'
        GIT_PASSWORD= 'GIT_PASSWORD'
        SONAR_SERVER= 'sonar-scanner'
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'Github-Credentials', 
                url: 'https://github.com/Hiransanjeewa/cicd-end-to-end',
                branch: 'main'
            }
        } 

        stage('Static Code Analysis') {
          environment {
            SONAR_URL = "http://34.27.30.176:9000"
          }
          steps {
            withCredentials([string(credentialsId: 'sonarqube', variable: 'SONAR_AUTH_TOKEN')]) {
              //   sh 'mvn sonar:sonar -Dsonar.login=$SONAR_AUTH_TOKEN -Dsonar.host.url=${SONAR_URL}'
              sh 'docker run -e SONAR_HOST_URL=${SONAR_URL} -e SONAR_TOKEN=$SONAR_AUTH_TOKEN -v $PWD:/code -w /code python:3.8 sonar-scanner'

            }
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
                url: 'https://github.com/Hiransanjeewa/Audiohub-Kubernetes-manifest.git',
                branch: 'main'
            }
        }

        stage('Update K8S manifest & push to Repo') {
            environment {
                GIT_REPO_NAME = "Audiohub-Kubernetes-manifest"
                GIT_USER_NAME = "Hiransanjeewa"
            }
            steps {
                script {
                    withCredentials([string(credentialsId: 'Github-Token', variable: 'GITHUB_TOKEN')]) {

                        sh '''
                        
                        
                        ls
                        pwd
                        cd manifests
                        chmod +rwx deploy.yml
                        sed -i "s/django:[0-9]*/django:${BUILD_NUMBER}/g" deploy.yml  
                        cat deploy.yml
                        cd ../
                        git status
                        git add .
                        git commit -m 'Updated the deploy yml | Jenkins Pipeline'
                        git remote -v
                        git push https://${GITHUB_TOKEN}@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME} HEAD:main





                        '''                        
                    }
                }
            }
        }
    }
}



// Add github token to credentials ID = Github-Token
//    sed -i -e "s/django:1/django:${BUILD_NUMBER}/g" deploy.yaml


   