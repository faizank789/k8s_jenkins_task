pipeline {
    agent any
//    triggers {
//        cron('* * * * *')
//    }
    environment {
        aws_account_id="062746389614"
        aws_default_region="us-east-1"
        image_repo_name="demo-test"
        repo_uri="${env.aws_account_id}.dkr.ecr.${env.aws_default_region}.amazonaws.com/${env.image_repo_name}"
        image_tag="latest"
        pass=credentials('login_cred')

        logging_ecr=true
        Build_image=true
        pushing_ecr=true
        deploy_k8s=true
    }

    stages {
        stage('Cloning git') {
            steps {
              checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: '278acc58-8382-432e-95f9-8827baee470d', url: 'git@github.com:faizank789/k8s_jenkins_task.git']]])
            }
        }

        stage('logging to ECR') {
            steps {
            script {
                if(env.logging_ecr == 'true') {
                try {
                  sh  "aws ecr get-login-password --region ${aws_default_region} | docker login --username AWS --password-stdin ${env.aws_account_id}.dkr.ecr.${env.aws_default_region}.amazonaws.com"       
                }
                catch (Exception errorlogs) {
                    println(errorlogs)
                    echo "Registry login issue Please check !"
                }
            }
            else {
                echo "Skipping Logging ECR !"
            }
            }
        }
    }

        stage('Building image') {
            steps {
             script {
                if (env.Build_image == 'true') {
                try {
                DockerImage = docker.build "${env.image_repo_name}:${env.image_tag}"
                }
                catch (Exception errorlogs) {
                    println(errorlogs)
                    echo "Docker image Building issue please check !" 
                }
             }
             else {
                echo "Skipping Building image !"
             }  
            }       
        }
    }

        stage('Pushing to ECR') {
            steps {
             script {
                if(env.pushing_ecr == 'true') {
                try {
                 sh "docker tag ${env.image_repo_name}:${env.image_tag} ${env.repo_uri}:${env.image_tag}"
                 sh "docker push ${env.aws_account_id}.dkr.ecr.${env.aws_default_region}.amazonaws.com/${env.image_repo_name}:${env.image_tag}"
                }
                catch (Exception errorlogs) {
                println (errorlogs)
                echo " ECR pushing issue please check !"

                }
             }
             else {
                echo "Skipping pushing ECR !"
             }
            }
        }
    }

        stage ('Deploying on k8s cluster') {
            steps {
                script {
                    if(env.deploy_k8s == 'true') {
            kubernetesDeploy(
                configs: 'deployment.yaml',
                kubeconfigId: 'config'
            )
                }
                else {
                    echo "skipping deployment !"
                }                
            }
        }
        }
    }
} 
