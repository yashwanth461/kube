pipeline {
    agent { label 'slave' }
    
    tools {
        maven 'maven' // Ensure Maven is installed on the Jenkins agent
    }

    environment {
        DOCKER_IMAGE = "yaeshvann/gtech:${BUILD_NUMBER}" 
        GIT_CREDENTIALS_ID = "gittoken" 
        GIT_REPO_NAME = "gctech"
        GIT_USER_NAME = "yashwanth461"

        

    }

    stages {
        stage('Checkout Git Repository') {
            steps {
               
                git branch: 'main', credentialsId: GIT_CREDENTIALS_ID, url: 'https://github.com/yashwanth461/gctech.git'
            }
        }
        
        stage('Build with Maven') {
            steps {
                sh 'mvn clean package' 
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    
                    sh "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }

        stage('Push Docker Image to Registry') {
            steps {
                script {
                    // Push the Docker image to the specified Docker registry
                    withDockerRegistry(credentialsId: 'yashid') {
                        sh "docker push ${DOCKER_IMAGE}"
                    }
                }
            }
        }
        
        stage('Update Image Tag in deploy.yml') {
            steps {
                script {
                    
                     sh '''
                    git config user.name "yashwanth461"
                    git config user.email "yashwanthkatta31@gmail.com"
                    '''
                     
                    sh "sed -i 's|image: .*|image: ${DOCKER_IMAGE}|' deploy.yml"

                   

                   
                    withCredentials([string(credentialsId: 'gittoken', variable: 'GITHUB_TOKEN')]) {
                        
                        sh '''
                        git add deploy.yml
                        git commit -m "Update deploy.yml with new Docker image: ${DOCKER_IMAGE}"
                        git push https://${GITHUB_TOKEN}@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME} HEAD:main
                       
                        '''
                    }
                }
            }
        }
    }

   
}

