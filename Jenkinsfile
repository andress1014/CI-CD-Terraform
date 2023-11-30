pipeline {
    parameters {
        booleanParam(name: 'autoApprove', defaultValue: false, description: 'Automatically run apply after generating plan?')
    } 
    environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
        TERRAFORM_PATH        = '/path/to/terraform'  // Update this path
    }

    agent any
    stages {
        stage('Install Terraform') {
            steps {
                script {
                    sh "curl -O https://releases.hashicorp.com/terraform/X.Y.Z/terraform_X.Y.Z_linux_amd64.zip"
                    sh "unzip terraform_X.Y.Z_linux_amd64.zip -d ${env.TERRAFORM_PATH}"
                    sh "export PATH=$PATH:${env.TERRAFORM_PATH}"
                }
            }
        }

        stage('checkout') {
            steps {
                script {
                    dir("terraform") {
                        git "https://github.com/andress1014/CI-CD-Terraform.git"
                    }
                }
            }
        }

        stage('Plan') {
            steps {
                script {
                    sh 'pwd; cd terraform/ ; terraform init'
                    sh "pwd; cd terraform/ ; terraform plan -out tfplan"
                    sh 'pwd; cd terraform/ ; terraform show -no-color tfplan > tfplan.txt'
                }
            }
        }

        stage('Approval') {
            when {
                not {
                    equals expected: true, actual: params.autoApprove
                }
            }

            steps {
                script {
                    def plan = readFile 'terraform/tfplan.txt'
                    input message: "Do you want to apply the plan?",
                    parameters: [text(name: 'Plan', description: 'Please review the plan', defaultValue: plan)]
                }
            }
        }

        stage('Apply') {
            steps {
                sh "pwd; cd terraform/ ; terraform apply -input=false tfplan"
            }
        }
    }
}
