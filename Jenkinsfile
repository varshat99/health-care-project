pipeline{
    agent any
    tools {
        git 'git'
        maven 'maven'
    }    
    stages{
        stage('checkout the code from github'){
            steps{
                 git url: 'https://github.com/varshat99/health-care-project.git'
                 echo 'github url checkout'
            }
        }
        stage('codecompile'){
            steps{
                echo 'starting compiling'
                sh 'mvn compile'
            }
        }
        stage('codetesting'){
            steps{
                sh 'mvn test'
            }
        }
        stage('qa'){
            steps{
                sh 'mvn checkstyle:checkstyle'
            }
        }
        stage('package'){
            steps{
                sh 'mvn package'
            }
        }
        stage('run dockerfile'){
          steps{
               sh 'docker build -t myimg1 .'
           }
         }
        stage('port expose'){
            steps{
                sh 'docker run -dt -p 8082:8082 --name c001 myimg1'
            }
        } 
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    echo 'Deploying DockerHub image to Kubernetes...'
                }
                withKubeConfig([credentialsId: 'kubeconfig-cred']) {
                    sh '''
                        echo "Applying Kubernetes deployment and service..."
                        kubectl apply -f k8s/deployment.yaml
                        kubectl apply -f k8s/service.yaml
                        kubectl rollout status deployment/healthcare-deployment
                    '''
                }
            }
    }
}
