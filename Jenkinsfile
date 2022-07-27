pipeline {
    agent any
    triggers {
        cron('* * * * *')
    }
    environment {
        aws_account_id="062746389614"
        aws_default_region="us-east-1"
        image_repo_name="demo-test"
        repo_uri="${env.aws_account_id}.dkr.ecr.${env.aws_default_region}.amazonaws.com/${env.image_repo_name}"
        image_tag="latest"
        pass=credentials('login_cred')
    }

    stages {
        stage('logging to ECR') {
            steps {
            script {
                try {
             sh "aws ecr --region us-east-1 | docker login -u AWS -p ${pass} ${env.aws_account_id}.dkr.ecr.${env.aws_default_region}.amazonaws.com"
                }
                catch (Exception errorlogs) {
                    println(errorlogs)
                }
            }
            }
        }
        stage('Cloning git') {
            steps {
              checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: '278acc58-8382-432e-95f9-8827baee470d', url: 'git@github.com:faizank789/k8s_jenkins_task.git']]])
            }
        }
        stage('Building image') {
            steps {
             script {
                try {
                DockerImage = docker.build "${env.image_repo_name}:${env.image_tag}"
                }
                catch (Exception errorlogs) {
                    println(errorlogs)
                }
             }  
            }       
        }
        stage('Pushing to ECR') {
            steps {
             script {
                try {
                 sh "docker tag ${env.image_repo_name}:${env.image_tag} ${env.repo_uri}:${env.image_tag}"
                 sh "docker push ${env.aws_account_id}.dkr.ecr.${env.aws_default_region}.amazonaws.com/${env.image_repo_name}:${env.image_tag}"
                }
                catch (Exception errorlogs)
                println (errorlogs)
             }
            }
        }
    }
}






