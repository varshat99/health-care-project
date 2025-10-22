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
                sh 'docker run -dt -p 8096:8082 --name c016 myimg1'
            }
        } 
        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: 'kubeconfig-file', variable: 'KUBECONFIG_FILE')]){
                // Write kubeconfig content to a file Jenkins can use
                sh '''
                    echo "using kubeconfig file from jenkins credentials"
                    export KUBECONFIG=$KUBECONFIG_FILE
                    kubectl get nodes
                    kubectl apply -f k8.yml
                    kubectl get pods
                '''
                }
            
            }
    }
}
}
