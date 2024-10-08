pipeline {
    agent {
        label 'My-Jenkins-Agent'
    }
    // agent any
    environment {
        APP_NAME = "devops-003-pipeline-aws"
        RELEASE = "1.0"
        DOCKER_USER = "ademk7604"
        DOCKER_LOGIN = "dockerhub"
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}.${BUILD_NUMBER}"
    }
    tools {
        jdk 'JDK21'
        maven 'Maven3'
    }
    stages {
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Checkout from SCM') {
            steps {
                //   checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/ademk7604/devops-003-pipeline-aws']])
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/ademk7604/devops-003-pipeline-aws'
            }
        }
        stage('Build Maven') {
            steps {
                //  sh 'mvn clean install'
                //  bat 'mvn clean install'
                sh 'mvn clean package'
                //  bat 'mvn clean package'
            }
        }
        stage('Test Application') {
            steps {
                sh 'mvn test'
                //  bat 'mvn test'
            }
        }
        stage("SonarQube Analysis") {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token') {
                        sh "mvn sonar:sonar"
                    }
                }
            }
        }

 /*
       stage("Quality Gate"){
           steps {
               script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'
                }
            }
        }
*/

        stage('Build & Push Docker Image to DockerHub') {
            steps {
                script {
                    docker.withRegistry('', DOCKER_LOGIN) {
                        docker_image = docker.build "${IMAGE_NAME}"
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push("latest")
                    }
                }
            }
        }
        stage("Trivy Scan") {
            steps {
                script {
                    sh ('docker run -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image ademk7604/devops-003-pipeline-aws:latest --no-progress --scanners vuln  --exit-code 0 --severity HIGH,CRITICAL --format table')
                }
            }
        }
        stage ('Cleanup Artifacts') {
            steps {
                script {
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"

                    // Kullanılmayan Docker volume'leri temizle
                    //sh "docker volume prune -f"

                    // Kullanılmayan Docker container'ları temizle
                    // sh "docker container prune -f"

                    // Genel sistem temizliği (image, container, volume, network)
                    // sh "docker system prune -a -f"
                }
            }
        }

        /*
        stage('Deploy to Kubernetes'){
            steps{
                kubernetesDeploy (configs: 'deployment-service.yml', kubeconfigId: 'kubernetes')
            }
        }


       stage('Docker Image to Clean') {
           steps {
               //   sh 'docker rmi ademk7604/my-application:latest'
               //  bat 'docker rmi ademk7604/my-application:latest'

               // sh 'docker image prune -f'
                bat 'docker image prune -f'
           }
       }
*/
    }
}