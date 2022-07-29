pipeline {
    agent any
//    triggers {
//        cron('* * * * *')
//    }
    environment {
        aws_account_id="06274638961"
        aws_default_region="us-east-1"
        image_repo_name="demo-test"
        repo_uri="${env.aws_account_id}.dkr.ecr.${env.aws_default_region}.amazonaws.com/${env.image_repo_name}"
        image_tag="latest"
        pass=credentials('login_cred')
    }

    stages {
        // stage('Cloning git') {
        //     steps {
        //       checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: '278acc58-8382-432e-95f9-8827baee470d', url: 'git@github.com:faizank789/k8s_jenkins_task.git']]])
        //     }
        // }

        // stage('logging to ECR') {
        //     steps {
        //     script {
        //         try {
        //      sh "aws ecr --region us-east-1 | docker login -u AWS -p ${pass} ${env.aws_account_id}.dkr.ecr.${env.aws_default_region}.amazonaws.com"
        //         }
        //         catch (Exception errorlogs) {
        //             println(errorlogs)
        //             echo "Registry login issue Please check !"
        //         }
        //     }
        //     }
        // }

        // stage('Building image') {
        //     steps {
        //      script {
        //         try {
        //         DockerImage = docker.build "${env.image_repo_name}:${env.image_tag}"
        //         }
        //         catch (Exception errorlogs) {
        //             println(errorlogs)
        //             echo "Docker image Building issue please check !" 
        //         }
        //      }  
        //     }       
        // }

        // stage('Pushing to ECR') {
        //     steps {
        //      script {
        //         try {
        //          sh "docker tag ${env.image_repo_name}:${env.image_tag} ${env.repo_uri}:${env.image_tag}"
        //          sh "docker push ${env.aws_account_id}.dkr.ecr.${env.aws_default_region}.amazonaws.com/${env.image_repo_name}:${env.image_tag}"
        //         }
        //         catch (Exception errorlogs) {
        //         println (errorlogs)
        //         echo " ECR pushing issue please check !"

        //         }
        //      }
        //     }
        // }

        stage ('Deploying on k8s cluster') {
            steps {
                script {
                    try {
                        sh "kubectl apply -f ${env.workspace}/configmap.yaml --token k8s-aws-v1.aHR0cHM6Ly9zdHMudXMtZWFzdC0xLmFtYXpvbmF3cy5jb20vP0FjdGlvbj1HZXRDYWxsZXJJZGVudGl0eSZWZXJzaW9uPTIwMTEtMDYtMTUmWC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBUTVHN1o1UlhPSk1CV0lPRyUyRjIwMjIwNzI5JTJGdXMtZWFzdC0xJTJGc3RzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyMjA3MjlUMTEwMjMzWiZYLUFtei1FeHBpcmVzPTYwJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCUzQngtazhzLWF3cy1pZCZYLUFtei1TaWduYXR1cmU9MDQ2YjIwNmM0MWY2YWY0NmZhMGU0NDNlNjY5NWM2N2RlNzI5ZGViNDEyMGE3ODc0MzZmNzg1ZjVmMWM4ZTBhOA --server https://9155A2802D4FF46CBD0067368575B739.gr7.us-east-1.eks.amazonaws.com"
                        sh "kubectl apply -f ${env.workspace}/deployment.yaml --token k8s-aws-v1.aHR0cHM6Ly9zdHMudXMtZWFzdC0xLmFtYXpvbmF3cy5jb20vP0FjdGlvbj1HZXRDYWxsZXJJZGVudGl0eSZWZXJzaW9uPTIwMTEtMDYtMTUmWC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBUTVHN1o1UlhPSk1CV0lPRyUyRjIwMjIwNzI5JTJGdXMtZWFzdC0xJTJGc3RzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyMjA3MjlUMTEwMjMzWiZYLUFtei1FeHBpcmVzPTYwJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCUzQngtazhzLWF3cy1pZCZYLUFtei1TaWduYXR1cmU9MDQ2YjIwNmM0MWY2YWY0NmZhMGU0NDNlNjY5NWM2N2RlNzI5ZGViNDEyMGE3ODc0MzZmNzg1ZjVmMWM4ZTBhOA --server https://9155A2802D4FF46CBD0067368575B739.gr7.us-east-1.eks.amazonaws.com"
                }
                catch (Exception errorlogs) {
                println (errorlogs)
                echo " Issue on Deployment on K8s Please check !"
                }
                } 
                 }     }
    } 
}
