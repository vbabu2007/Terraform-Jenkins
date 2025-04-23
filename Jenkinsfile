pipeline {
    agent any

    environment {
        TF_VERSION = '1.6.0' // Optional: set your required Terraform version
        TF_WORKSPACE = 'default'
    }

    tools {
        terraform "${TF_VERSION}"
    }

    stages {
        stage('Clone GitHub Repo') {
            steps {
                git credentialsId: 'vbabu2007', url: 'https://github.com/vbabu2007/Terraform-Jenkins.git', branch: 'main'
            }
        }

        stage('Terraform Init') {
            steps {
                sh 'terraform init'
            }
        }

        stage('Terraform Validate') {
            steps {
                sh 'terraform validate'
            }
        }

        stage('Terraform Plan') {
            steps {
                sh 'terraform plan -out=tfplan'
            }
        }

        stage('Terraform Apply') {
            when {
                expression {
                    return params.APPLY_TERRAFORM == true
                }
            }
            steps {
                sh 'terraform apply -auto-approve tfplan'
            }
        }
    }

    parameters {
        booleanParam(name: 'APPLY_TERRAFORM', defaultValue: false, description: 'Apply Terraform Plan?')
    }

    post {
        always {
            echo "Pipeline finished"
        }
    }
}
