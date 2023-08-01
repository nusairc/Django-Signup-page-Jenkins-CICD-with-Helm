pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'env-variable for git', url: 'github repo url']])
            }
        }
        stage('Python Build') {
            steps {
                dir('./registration'){    
                 bat 'python settings.py build'
                }
            }}

       stage('Unit Test') {
            steps {
                dir('./registration/app1') {
                    bat 'python ../../manage.py test'
                }
            }
        }

            
        stage('Docker Login and Build') {
            steps {
                withCredentials([string(credentialsId: 'dockerusername', variable: 'docker-cred-variable')]) {
                    bat 'docker login -u username -p %docker-cred-variable%'
                    bat "docker build -t docker-imagename:${env.BUILD_NUMBER} . "
                    bat "docker push docker-image name:${env.BUILD_NUMBER}"
                    bat 'docker logout'
                }
            }
        }


        stage('helmChart tag') {
            steps {
                // bat "sed -i 's|docker-image name:v1|docker-image name:${env.BUILD_NUMBER}|g' ./signup-chart/values.yaml"
                // Here i used command for powershell , modify according tp your OS
                bat """
                powershell.exe -Command "((Get-Content -Path './signup-chart/values.yaml') -replace 'image-name-tag', 'image-name:${env.BUILD_NUMBER}') | Set-Content -Path './signup-chart/values.yaml'"
                """
            }
        }
        
        stage('helm package ') {
            steps {
                //here i used bat command for windows with direct access from path in my local machine , modify according to your requirements
                bat "\"C:\\Program Files\\windows-amd64\\helm\" package E:\\Signup-pro\\registration\\signup-chart"
                // bat 'wsl /usr/local/bin/helm package signup-chart
                // bat 'wsl helm package signup-chart'
                // bat 'wsl sudo helm package signup-chart'
                
            }
        }

        stage('Logging into AWS ECR & push helm chart to ECR') {
            steps {
                withCredentials([aws(credentialsId: 'aws-credentails-variable', accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    script {
                        bat '"C:\\Program Files\\Amazon\\AWSCLIV2\\aws" ecr get-login-password --region us-east-1 | "C:\\Program Files\\windows-amd64\\helm" registry login --username AWS --password-stdin 947437598996.dkr.ecr.us-east-1.amazonaws.com'
                        bat "\"C:\\Program Files\\windows-amd64\\helm\" push signup-chart-0.1.0.tgz oci://947####996.dkr.ecr.us-east-1.amazonaws.com"
                        bat "del signup-chart-0.1.0.tgz"
                        //here i used aws credential and windows path , modify according to your requriments
                    }
                }
            }
        }

        
         stage('pass buildnumber to another pipeline') {
            steps {
                build job: 'helm2-pipeline', parameters: [string(name: 'build_number', value: "${build_number}")]
            }
        }
    }
}
