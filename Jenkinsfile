#!/usr/bin/env groovy

env.ARTIFACT_LOCATION = "/output/${env.BUILD_TAG}"
env.ENVIRONMENT = 'ci'

try {

    currentBuild.result = 'SUCCESS'

    node ('ecs-java') {


       
        milestone 100
       // input "Deploy to ${ENVIRONMENT}?"
        milestone 110
		
		  stage ('Checkout') {
		  checkout scm
		  }

		stage ('Environment Deployment') {
            sh "curl https://releases.hashicorp.com/terraform/0.11.7/terraform_0.11.7_linux_amd64.zip?_ga=2.189758636.649883576.1524444123-2051905022.1524067823 -o terraform_0.11.7_linux_amd64.zip"
            sh "unzip terraform_0.11.7_linux_amd64.zip"
            sh "rm terraform_0.11.7_linux_amd64.zip"
            sh "chmod +x ./terraform" 
            sh "chmod 775 /home/jenkins/workspace/core-infra"
            sh "cd /home/jenkins/workspace/core-infra"
            sh "ls -l"
            
            
        }
        stage ('Terraform Deployment') {
            crossAccountAndDeploy("${ENVIRONMENT}")
        }
        crossAccountAndDeploy("${ENVIRONMENT}")
        stage ('Terraform apply') {
              //milestone 120
            sh "/home/jenkins/workspace/core-infra/terraform apply /home/jenkins/workspace/core-infra/tf.plan -input=false -auto-approve"
            input(message: "destroy?", ok: "destroy")  
        }
        stage ('Terraform destroy') {
              //milestone 120
            sh "/home/jenkins/workspace/core-infra/terraform destroy -force"
        } 
        stage ('cleanup') {
			sh "make AWS_REGION=us-east-1 clean; make AWS_REGION=us-east-1 clean-plan"			
        }
    }
}
catch (err) {
    currentBuild.result = 'FAILURE'
    throw err
}

def crossAccountAndDeploy(env) {
	sh "sh /home/jenkins/workspace/core-infra/jenkins-deploy.sh '${env}' '.' 'us-east-1' ${BUILD_TAG}"
    sh "/home/jenkins/workspace/core-infra/terraform init"
    sh "/home/jenkins/workspace/core-infra/terraform plan -out /home/jenkins/workspace/core-infra/tf.plan"
    input(message: "Promote?", ok: "Promote")            
}
