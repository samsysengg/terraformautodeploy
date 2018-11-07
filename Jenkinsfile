#!/usr/bin/env groovy
stage ('Checkout') {
checkout scm
}
		stage ('Environment Deployment') {
            sh "curl https://releases.hashicorp.com/terraform/0.11.7/terraform_0.11.7_linux_amd64.zip?_ga=2.189758636.649883576.1524444123-2051905022.1524067823 -o terraform_0.11.7_linux_amd64.zip"
            sh "unzip terraform_0.11.7_linux_amd64.zip"
            sh "rm terraform_0.11.7_linux_amd64.zip"
            sh "chmod +x ./terraform" 
	    sh "rm ./terraform"
	    sh "pwd"
            
        }
