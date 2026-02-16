pipeline{
    agent any
<<<<<<< HEAD
=======
    tools{
        jdk 'jdk21'
        nodejs 'node16'
    }
    environment {
        SCANNER_HOME=tool 'sonar-scanner'
    }
>>>>>>> ec18f6affc5c7b44366cf1f9919cba5822f23d1f
    stages {
        stage('Checkout from Git'){
            steps{
                git branch: 'master', url: 'https://github.com/vijay3639/Chat-gpt.git'
            }
        }
<<<<<<< HEAD
        stage('Terraform version'){
             steps{
                 sh 'terraform --version'
             }
        }
        stage('Terraform init'){
             steps{
                 dir('Eks-terraform') {
                      sh 'terraform init'
                   }
             }
        }
        stage('Terraform validate'){
             steps{
                 dir('Eks-terraform') {
                      sh 'terraform validate'
                   }
             }
        }
        stage('Terraform plan'){
             steps{
                 dir('Eks-terraform') {
                      sh 'terraform plan'
                   }
             }
        }
        stage('Terraform apply/destroy'){
             steps{
                 dir('Eks-terraform') {
                      sh 'terraform ${action} --auto-approve'
                   }
             }
        }
    }
}
=======
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
        stage("Sonarqube Analysis "){
            steps{
                withSonarQubeEnv('sonar-server') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Chatbot \
                    -Dsonar.projectKey=Chatbot '''
                }
            }
        }
        stage("quality gate"){
           steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token'
                }
            }
        }
        stage('OWASP FS SCAN') {
            environment {
                NVD_API_KEY = credentials('NVD_API_KEY')
            }
            steps {
                dependencyCheck additionalArguments: "--scan ./ --disableYarnAudit --disableNodeAudit --nvdApiKey=${NVD_API_KEY}", odcInstallation: 'DP-Check'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        stage('TRIVY FS SCAN') {
            steps {
                sh "trivy fs . > trivyfs.json"
            }
        }
        stage("Docker Build & Push"){
            steps{
                script{
                   withDockerRegistry(credentialsId: 'docker', toolName: 'docker'){
                       sh "docker build -t chatbot ."
                       sh "docker tag chatbot vijay3639/chatbot:latest "
                       sh "docker push vijay3639/chatbot:latest "
                    }
                }
            }
        }
        stage("TRIVY"){
            steps{
                sh "trivy image vijay3639/chatbot:latest > trivy.json"
            }
        }
        stage ("Remove container") {
            steps{
                sh "docker stop chatbot | true"
                sh "docker rm chatbot | true"
             }
        }
        stage('Deploy to container'){
            steps{
                sh 'docker run -d --name chatbot -p 3000:3000 vijay3639/chatbot:latest'
            }
        }
    }
}


>>>>>>> ec18f6affc5c7b44366cf1f9919cba5822f23d1f
